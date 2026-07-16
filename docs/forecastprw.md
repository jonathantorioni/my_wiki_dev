---
id: forecastprw
title: FORECAST.PRW
---

# FORECAST.PRW

**Analista:** Jonathan Everton Torioni Oliveira

----

## Visão geral

Este módulo fornece a interface de navegação (**browser**) para o gerenciamento e visualização de previsões (**forecast**) no sistema Protheus. Através de um browser customizado (`mBrowse`), o usuário pode:

- Visualizar registros da tabela **YA7** (Forecast).
- Gerar previsões a partir de parâmetros de negócio.
- Emitir relatórios de forecast.
- Excluir forecasts já criados.

A interface utiliza legendas de cores baseadas no campo `YA7_STATUS` para indicar o status de cada registro:

- **Verde** – `YA7_STATUS = '1'`
- **Vermelho** – `YA7_STATUS = '2'`

A mensagem padrão de tabela (`_lMsgTabela`) está desativada para evitar ruído na interface.

----

## Funções principais

| Função | Tipo | Descrição |
|--------|------|-----------|
| `FORECAST()` | **User Function** | Inicia o browser de Forecast, configurando cores e menu. |
| `MenuDef()` | **Static Function** | Define as opções de menu exibidas no browser. |
| `GERFCAST(oSay)` | **User Function** | Monta parâmetros para a geração do forecast e chama `EXEFCAST()`. |
| `EXEFCAST()` | **Static Function** | Executa o processamento de forecast usando parâmetros definidos. |
| `FCDIAS(nMeses)` | **User Function** | Gera array com datas de simulação do forecast. |
| `RFILDIS()` | **Static Function** | Retorna as filiais distribuidoras. |
| `FDIACOM()` | **User Function** | Exibe tela de seleção de dias de compra futura. |
| `YA7YA8D()` | **User Function** | Exclui o forecast selecionado no browser. |
| `RUNFUN1()` | **User Function** | Executa a geração do forecast (chama `U_GERFCAST`). |
| `RUNFUN2()` | **User Function** | Executa a geração do relatório de forecast (chama `U_RELFOREC`). |
| `RETCODNV(cProd)` | **Static Function** | Recupera códigos de produto novos a partir de histórico. |

### `FORECAST()`
```advpl
User Function FORECAST()
    Private aRotina := MenuDef()
    Private _cFilBrw := ""
    Private cCadastro := "Forecast"
    Private aCores := {}
    PUBLIC _lMsgTabela := .F.
    aAdd(aCores,{"YA7_STATUS == '1' ", "BR_VERDE" })
    aAdd(aCores,{"YA7_STATUS == '2' ", "BR_VERMELHO" })
    mBrowse( 6,1,22,75,"YA7",,,,,,aCores,,,,.T., ,_cFilBrw)
RETURN
```
- Configura o browser `mBrowse` para a tabela **YA7**.
- As cores são definidas em `aCores`.
- O menu é fornecido por `MenuDef()`.


### `MenuDef()`
```advpl
STATIC FUNCTION MenuDef()
    Local aRotina:= {}
    aAdd(aRotina,{"Pesquisar","AxPesqui",0,1})
    aAdd(aRotina,{"Gerar Forecast","U_RUNFUN1",0,3})
    aAdd(aRotina,{"Relatorio","U_RUNFUN2",0,2})
    aAdd(aRotina,{"Excluir","U_YA7YA8D()",0,5})
RETURN aRotina
```
Define quatro opções de menu: **Pesquisar**, **Gerar Forecast**, **Relatório**, **Excluir**.

### `GERFCAST(oSay)`
Responsável por montar a tela de parâmetros de geração do forecast. Usa `U_SHPRMBOX` para coletar valores e, em seguida, chama `EXEFCAST()`.

### `EXEFCAST()`
Realiza a lógica de negócio principal:
1. Busca dados de vendas e produto via query SQL.
2. Calcula a confiança (`aConfia`) e simula consumo futuro.
3. Insere registros temporários na tabela **YA8** para cada período simulado.
4. Atualiza a tabela **YA7** com os parâmetros fornecidos.

> **Nota:** A rotina inclui validações de campos obrigatórios (fornecedor, dias de compra futura) e mensagens de erro usando `FWAlertError`.

----

## Fluxo típico de uso
1. **Abrir a rotina** – Execute a função `FORECAST()` a partir do menu do Protheus.
2. **Selecionar ação** – No menu ao lado, escolha **Gerar Forecast** ou **Relatório**.
3. **Preencher parâmetros** – A tela exibida por `GERFCAST` solicita data de referência, grupo, fornecedor, dias de compra futura, etc.
4. **Processamento** – Uma barra de progresso aparece (`FwMsgRun`) enquanto `EXEFCAST` gera as previsões.
5. **Visualizar resultados** – O browser exibe os registros atualizados com cores indicando o status.
6. **Excluir (opcional)** – Se necessário, selecione um registro e escolha **Excluir**; a rotina verifica se o forecast está firmado antes de remover.

----

## Requisitos de ambiente
- Tabelas referenciadas (`YA7`, `YA8`, `SB1`, `SA2`, `PD8`).
- Funções auxiliares externas (`U_SHPRMBOX`, `U_GERFCAST`, `U_RELFOREC`, `MpSysOpenQuery`, `RetSqlName`, `ProdutoBOConsultaDAO`, etc.) devem estar disponíveis no ambiente.

----

## Dicas de homologação
1. Verificar cores do browser (verde/vermelho).
2. Confirmar mensagens de progresso **"Gerando Forecast..."** e **"Montando relatório..."**.
3. Validar geração correta da tabela **YA8** após a simulação.
4. Testar exclusão somente de forecasts não firmados.
