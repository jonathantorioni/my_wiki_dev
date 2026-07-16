---
id: mpswcompprw
title: MPSWCOMP.PRW
---

# MPSWCOMP.PRW

**Controle de Acesso e Restrição de Compartilhamento de Senhas**

**Programa:** MPSWCOMP.PRW  
**Versão:** 1.1  
**Data:** 14/04/2026  
**Autor:** Jonathan Torioni

---

## Histórico de Versões

| Versão | Data | Alteração |
|---|---|---|
| 1.0 | 13/04/2026 | Versão inicial — controle de acesso por máquina no AfterLogin |
| 1.1 | 14/04/2026 | Adição da rotina administrativa `ZACADMIN` para o time de Suporte Protheus|

---

## 1. Objetivo

Impedir o compartilhamento de senhas no ERP TOTVS Protheus através do controle de máquinas autorizadas por usuário. O sistema registra e valida o hostname da estação no momento do login, permitindo apenas acessos originados de máquinas previamente cadastradas (Principal ou Secundária).

---

## 2. Constantes (`#Define`)

Definidas no topo do fonte, antes de qualquer função:

| Constante | Valor | Uso |
|---|---|---|
| `ENTER` | `Chr(13) + Chr(10)` | Quebra de linha em todas as mensagens do fonte |

---

## 3. Tabelas Utilizadas

### ZAA — Máquinas Cadastradas


Armazena as máquinas autorizadas para cada usuário.

| Campo | Tipo | Tam | Descrição |
|---|---|---|---|
| `ZAA_FILIAL` | Caracter | 2 | Filial da empresa |
| `ZAA_USER` | Caracter | 15 | ID do usuário (`__cUserId`) |
| `ZAA_HOST` | Caracter | 60 | Hostname da máquina |
| `ZAA_SO` | Caracter | 40 | Sistema operacional detectado |
| `ZAA_TIPO` | Caracter | 1 | Tipo: `P` = Principal / `S` = Secundária |
| `ZAA_DTCAD` | Data | 8 | Data de cadastro |
| `ZAA_HRCAD` | Caracter | 8 | Hora de cadastro |
| `ZAA_ATIVO` | Caracter | 1 | `S` = Ativo / `N` = Inativo |

**Índice único:** `ZAA_FILIAL + ZAA_USER + ZAA_HOST`

---

### ZAB — Auditoria de Acessos

Registra todos os eventos de acesso para fins de auditoria de segurança.

| Campo | Tipo | Tam | Descrição |
|---|---|---|---|
| `ZAB_FILIAL` | Caracter | 2 | Filial da empresa |
| `ZAB_USER` | Caracter | 15 | ID do usuário |
| `ZAB_HOST` | Caracter | 60 | Hostname da máquina |
| `ZAB_SO` | Caracter | 40 | Sistema operacional |
| `ZAB_TIPO` | Caracter | 1 | Tipo da máquina cadastrada |
| `ZAB_DATA` | Data | 8 | Data do evento |
| `ZAB_HORA` | Caracter | 8 | Hora do evento |
| `ZAB_STATUS` | Caracter | 1 | `L` = Liberado / `B` = Bloqueado / `C` = Cadastro |
| `ZAB_MSG` | Caracter | 200 | Descrição do evento |

**Índice:** `ZAB_FILIAL + ZAB_USER + ZAB_DATA + ZAB_HORA`

---

## 4. Fluxo Principal

```
AfterLogin (U_MPSWCOMP)
│
├── Usuário = ADMIN / 000000? ──────────────────────────────► Libera sem validação
│
├── Coleta hostname (ZAC_GtHost) e SO (ZAC_GetSO)
│
├── Máquina registrada e ativa na ZAA? (ZAC_MaqCad)
│   └── SIM ────────────────────────────────────────────────► Grava auditoria "L" → Libera acesso
│
├── Primeiro acesso do usuário? (ZAC_PrimAc — nenhuma máquina na ZAA)
│   └── SIM ─► ZAC_FlxPri
│               ├── FWAlertYesNo: Registrar como PRINCIPAL?
│               │   ├── SIM ─► Cadastra ZAA (tipo P) → auditoria "C" → Libera
│               │   └── NÃO ─► Auditoria "B" → Bloqueia
│
└── Usuário tem máquinas, mas esta não está registrada ─► ZAC_FlxNov
    │
    ├── Já tem Principal E Secundária? ──────────────────────► Auditoria "B" → Bloqueia
    │
    ├── FWAlertWarning (informativo: problema + soluções)
    │
    ├── Não tem Principal? ─► FWAlertYesNo: Registrar como PRINCIPAL?
    │                         ├── SIM ─► cTipo = "P"
    │                         └── NÃO ─► próxima pergunta
    │
    ├── cTipo ainda vazio? ─► FWAlertYesNo: Registrar como SECUNDÁRIA?
    │                         ├── SIM ─► cTipo = "S"
    │                         └── NÃO ─► próxima etapa
    │
    ├── cTipo preenchido? ─► Cadastra ZAA → auditoria "C" → Libera
    └── cTipo vazio?      ─► Auditoria "B" → Bloqueia

Acesso bloqueado:
  └── MsgStop (mensagem ao usuário) → __Quit()
```

---

## 5. Funções

### `User Function MPSWCOMP()`

**Tipo:** User function acionado pelo ponto de entrada (AfterLogin)  
**Descrição:** Função principal chamada automaticamente pelo Protheus após a autenticação do usuário. Orquestra todo o fluxo de validação de máquinas.

| Variável Local | Tipo | Descrição |
|---|---|---|
| `cUserId` | Caracter | ID do usuário (`AllTrim(__cUserId)`) |
| `cHostname` | Caracter | Hostname coletado da estação |
| `cSistOper` | Caracter | Sistema operacional da estação |
| `lAcesso` | Lógico | Resultado final da validação |

> **Exceção:** Usuários com ID `000000` ou nome `ADMINISTRADOR` são liberados sem validação para evitar lock-out acidental.

---

### `Static Function ZAC_GtHost()`

**Retorno:** `Caracter` — hostname em maiúsculas  
**Descrição:** Retorna o nome da estação cliente com três níveis de fallback:

1. `GetRmtInfo()[2]` — API nativa do SmartClient
2. `"HOST_DESCONHECIDO_" + timestamp` — identificador único de emergência

---

### `Static Function ZAC_GetSO()`

**Retorno:** `Caracter` — descrição do sistema operacional  
**Descrição:** Retorna o sistema operacional reportado pelo SmartClient via `GetRmtInfo()[4]`. Fallback: `"Windows (SmartClient)"`.

---

### `Static Function ZAC_MaqCad(cUserId, cHostname)`

**Retorno:** `Lógico` — `.T.` se a máquina está registrada e ativa  
**Descrição:** Consulta a tabela ZAA verificando se o hostname está cadastrado e ativo (`ZAA_ATIVO = 'S'`) para o usuário informado.

| Parâmetro | Tipo | Descrição |
|---|---|---|
| `cUserId` | Caracter | ID do usuário |
| `cHostname` | Caracter | Hostname da estação |

---

### `Static Function ZAC_PrimAc(cUserId)`

**Retorno:** `Lógico` — `.T.` se nenhuma máquina está cadastrada para o usuário  
**Descrição:** Conta os registros ativos na ZAA para o usuário. Retorna `.T.` se o total for zero (primeiro acesso).

| Parâmetro | Tipo | Descrição |
|---|---|---|
| `cUserId` | Caracter | ID do usuário |

---

### `Static Function ZAC_TmMaq(cUserId, cTipo)`

**Retorno:** `Lógico` — `.T.` se o usuário possui ao menos uma máquina do tipo informado  
**Descrição:** Verifica na ZAA se o usuário já possui uma máquina ativa do tipo especificado.

| Parâmetro | Tipo | Descrição |
|---|---|---|
| `cUserId` | Caracter | ID do usuário |
| `cTipo` | Caracter | `"P"` = Principal / `"S"` = Secundária |

---

### `Static Function ZAC_FlxPri(cUserId, cHostname, cSistOper)`

**Retorno:** `Lógico` — `.T.` se o acesso foi liberado  
**Descrição:** Fluxo de primeiro acesso. Exibe `FWAlertYesNo` solicitando que o usuário defina a máquina como Principal. Se confirmado, cadastra na ZAA e grava auditoria `"C"`. Se recusado, grava auditoria `"B"` e bloqueia.

| Parâmetro | Tipo | Descrição |
|---|---|---|
| `cUserId` | Caracter | ID do usuário |
| `cHostname` | Caracter | Hostname da estação |
| `cSistOper` | Caracter | Sistema operacional |

---

### `Static Function ZAC_FlxNov(cUserId, cHostname, cSistOper)`

**Retorno:** `Lógico` — `.T.` se o acesso foi liberado  
**Descrição:** Fluxo para usuário que já possui máquinas cadastradas mas acessa de uma nova estação. Regras:

- Se já possui **Principal e Secundária**: bloqueia imediatamente (limite atingido)
- Caso contrário: exibe `FWAlertWarning` informativo e pergunta sequencialmente via `FWAlertYesNo` se deseja registrar como Principal (se ainda não tiver) ou Secundária
- Se recusar todas as opções: bloqueia

| Parâmetro | Tipo | Descrição |
|---|---|---|
| `cUserId` | Caracter | ID do usuário |
| `cHostname` | Caracter | Hostname da estação |
| `cSistOper` | Caracter | Sistema operacional |

---

### `Static Function ZAC_CadMaq(cUserId, cHostname, cSistOper, cTipo)`

**Retorno:** `Lógico` — `.T.` após gravação  
**Descrição:** Insere um novo registro na tabela ZAA com `RecLock(.T.)` (append). Preenche todos os campos incluindo data/hora de cadastro e `ZAA_ATIVO = "S"`.

| Parâmetro | Tipo | Descrição |
|---|---|---|
| `cUserId` | Caracter | ID do usuário |
| `cHostname` | Caracter | Hostname da estação |
| `cSistOper` | Caracter | Sistema operacional |
| `cTipo` | Caracter | `"P"` = Principal / `"S"` = Secundária |

---

### `Static Function ZAC_RegAud(cUserId, cHostname, cSistOper, cTipo, cStatus, cDescricao)`

**Retorno:** `Nil`  
**Descrição:** Insere um registro de auditoria na tabela ZAB com `RecLock(.T.)` (append). Chamada em todos os eventos de acesso: liberado, bloqueado e cadastro.

| Parâmetro | Tipo | Descrição |
|---|---|---|
| `cUserId` | Caracter | ID do usuário |
| `cHostname` | Caracter | Hostname da estação |
| `cSistOper` | Caracter | Sistema operacional |
| `cTipo` | Caracter | Tipo da máquina (`"P"`, `"S"` ou `""`) |
| `cStatus` | Caracter | `"L"` = Liberado / `"B"` = Bloqueado / `"C"` = Cadastro |
| `cDescricao` | Caracter | Texto descritivo do evento |

---

---

### `User Function ZACADMIN()`

**Tipo:** Rotina de menu (chamada manualmente pelo usuário)
**Acesso:** Restrito ao grupo `GRP_SUPORTE` (`"SUPORTE"`)
**Descrição:** Tela administrativa para o time de Suporte Protheus. Exibe um menu com duas operações: cadastro manual de máquinas na ZAA e consulta read-only do log de auditoria (ZAB). Verifica o grupo do usuário antes de exibir qualquer interface.

**Fluxo:**
```
ZACADMIN
│
├── ZAC_AcAdm() ──────── usuário NÃO pertence ao grupo? ──► FWAlertWarning + Return
│
└── Aviso (menu de opcoes — multiplos botoes)
    ├── 1 = "Cadastrar Maquina para Usuario"  ──► ZAC_AdmCad()
    ├── 2 = "Consultar Log de Auditoria"      ──► ZAC_AdmAud()
    └── 3 = "Sair"                            ──► Return
```

---

### `Static Function ZAC_AcAdm()`

**Retorno:** `Lógico` — `.T.` se o usuário tem permissão
**Descrição:** Verifica se o usuário logado (`__cUserId`) pertence ao grupo `GRP_SUPORTE` via `FWGrpUser()`. O usuário `000000` sempre é autorizado (proteção anti-lock-out).

---

### `Static Function ZAC_AdmCad()`

**Retorno:** `Nil`
**Descrição:** Abre a tela padrão de cadastro do Protheus (`AxCadastro`) para a tabela ZAA. A interface é gerada automaticamente a partir do dicionário SX3, exibindo os campos conforme configurados pelo instalador (`U_ZACSENHAINST`). O time de suporte tem acesso às operações de inclusão, alteração e exclusão via interface padrão do Protheus.

**Sintaxe utilizada:**
```advpl
AxCadastro("ZAA", "Cadastro de Maquinas - Suporte Protheus", ".T.", ".T.")
```

| Parâmetro | Valor | Descrição |
|---|---|---|
| `cAlias` | `"ZAA"` | Tabela de máquinas cadastradas |
| `cTitulo` | `"Cadastro de Maquinas - Suporte Protheus"` | Título da janela |
| `cDel` | `".T."` | Exclusão sempre permitida |
| `cOk` | `".T."` | Inclusão/alteração sempre válida |

> A validação de negócio (tipo P/S, campos obrigatórios, unicidade) é garantida pelas definições do dicionário SX3 configuradas pelo instalador.

---

### `Static Function ZAC_AdmAud()`

**Retorno:** `Nil`
**Descrição:** Abre a tabela ZAB em modo de consulta read-only usando `MBrowse()` com `aAcao := {}` (array vazio), o que remove os botões de inclusão, alteração e exclusão da interface. Salva e restaura a área de trabalho com `GetArea()` / `RestArea()`.

> **Nenhuma gravação** é possível a partir desta função. O time de suporte pode apenas navegar e visualizar os registros de auditoria.

---

## 6. Regras de Negócio

| Situação | Comportamento |
|---|---|
| Usuário `000000` ou `ADMINISTRADOR` | Liberado sem validação |
| Primeiro acesso (nenhuma máquina na ZAA) | Solicita cadastro como Máquina Principal |
| Máquina já registrada e ativa | Liberado automaticamente |
| Nova máquina, sem Principal cadastrada | Oferece cadastro como Principal |
| Nova máquina, tem Principal mas não Secundária | Oferece cadastro como Secundária |
| Nova máquina, já tem Principal **e** Secundária | Bloqueado imediatamente |
| Recusa todas as opções de cadastro | Bloqueado |

---

## 7. Auditoria

Todos os eventos são gravados na tabela **ZAB** com status:

| Status | Significado |
|---|---|
| `L` | Acesso liberado — máquina registrada e ativa |
| `C` | Cadastro de nova máquina realizado pelo usuário |
| `B` | Acesso bloqueado — máquina não registrada ou limite atingido |

Os registros podem ser consultados diretamente via SQL:

```sql
SELECT ZAB_USER, ZAB_HOST, ZAB_SO, ZAB_DATA, ZAB_HORA, ZAB_STATUS, ZAB_MSG
FROM ZABG01
WHERE ZAB_FILIAL = ' '
ORDER BY ZAB_DATA DESC, ZAB_HORA DESC
```

---

## 8. Implantação

[Documento de implantação](../../docs/5-Processos/implantacao_mpswcomp.md)
