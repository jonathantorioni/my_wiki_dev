---
id: fismntnfeprw
title: FISMNTNFE.PRW
---

# FISMNTNFE.PRW

**Ponto de entrada do monitor de NF-e SEFAZ - auditoria de consultas e controle de "Refresh"**

----

### Dados da customização

Analista responsável: Jonathan Torioni

----

### Especificação da customização

Esta customização é um ponto de entrada disparado pelo monitor de NF-e SEFAZ logo após uma consulta de chave de acesso. Tem dois objetivos:

1. **Auditar a consulta** gravando o retorno da SEFAZ (código + mensagem) na tabela `LEGBRANCA`, utilizada para evitar a rejeição **656 - Consumo Indevido**. Quando o registro já existe, é apenas atualizado o timestamp e os campos de retorno.
2. **Controlar o uso do botão Refresh** no browse do monitor, impedindo que o usuário acione consultas em intervalos inferiores a 60 segundos. Caso o usuário tente, é exibido um diálogo modal com aviso e contador de 60 segundos.

----

### Especificação de tabelas e índices

* **LEGBRANCA** - Tabela customizada de auditoria de consultas SEFAZ (consultada e gravada via `U_INS656`, `U_CON656`, `U_STMPUPD`).

----

### Especificação de parâmetros

* **ES_AUDLEG** - Parâmetro lógico que habilita a auditoria/gravação na tabela `LEGBRANCA`. Quando `.F.`, o ponto de entrada retorna sem fazer nada.

----

### Especificação das funções

* **U_FISMNTNFE** - Ponto de entrada principal disparado pelo monitor `nfesefaz`. Recebe via `PARAMIXB` a chave consultada (doc/série) e o retorno da SEFAZ (`cCodRet`, `cRetSef`). Caso `ES_AUDLEG` esteja habilitado, valida via `U_CON656` se o registro já existe na `LEGBRANCA`. Em caso negativo grava o registro com `U_INS656`; em caso positivo atualiza com `U_STMPUPD`. Em seguida, identifica via `ProcName()` se a chamada veio do botão **Refresh** e, em caso afirmativo, valida o intervalo de 60 segundos entre cliques. Se a janela tiver sido aberta dentro do intervalo, exibe o modal de aguardo (`fismntmg`).

* **fismntmg** - Função estática que exibe um `FWDialogModal()` modal não fechável pelo usuário com a mensagem de aguardo. O modal é encerrado automaticamente após `nSegundos` via `SetTimer`. Recebe parâmetros opcionais `cMensagem`, `cTitulo` e `nSegundos` (default 3).

----

:::info

Este ponto de entrada utiliza a variável Static `cUltTempo` para manter o controle do último clique do Refresh entre execuções sucessivas dentro da mesma sessão.

:::

:::info

A identificação do clique no botão Refresh é feita inspecionando a pilha de execução via `ProcName()` procurando pela string `"ALISTBOX:=GETLISTBOX("`.

:::
