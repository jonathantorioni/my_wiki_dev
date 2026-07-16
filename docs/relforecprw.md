---
id: relforecprw
title: RELFOREC.PRW
---

# RELFOREC.PRW

**Analista:** Jonathan Everton Torioni Oliveira

----

## Visão geral

Este fonte gera o **relatório analítico de Forecast** e permite a exportação dos dados para Excel (XML). Ele consolida previsões de compra por período, fornecedor e mês, exibindo as informações em telas interativas (grids) e oferece a funcionalidade de **firmar** o forecast, travando os meses planejados para impedir alterações posteriores.

----

## Funções principais

| Função | Tipo | Descrição |
|--------|------|-----------|
| `RELFOREC(oSay)` | **User Function** | Monta o relatório, executa consultas SQL, preenche grids e exporta para Excel. |
| `fWBrowse1(_aHdPer,aWBrowse1)` | **Static** | Renderiza a grid **Período** (quantidades por classe de data). |
| `fWBrowse2(aHdMes,aMensal)` | **Static** | Renderiza a grid **Mensal** (valores por fornecedor/mês). |
| `fWBrowse3(aTotal,nTotal)` | **Static** | Renderiza a grid **Acumulado** (total por fornecedor). |
| `fWBrowse4(aHdQtd,aQtdFor)` | **Static** | Renderiza a grid **Fornecedor por Quantidade** (analítico). |
| `fWBrowse5(aHdVal,aValFor)` | **Static** | Renderiza a grid **Fornecedor por Valor** (analítico). |
| `EXPEXCEL(aCabs, cTipo, oSay)` | **Static** | Exporta os grids para planilhas Excel (XML) – diferentes layouts para analítico e gerencial. |
| `FIRMAFC()` | **Static** | “Faz o Forecast” – bloqueia meses selecionados, atualiza `YA7_FIRME` e `YA7_MFIRME`. |

### `RELFOREC(oSay)`
```advpl
User Function RELFOREC(oSay)
    // 1. Carrega período e cabeçalhos usando U_FOREMES()
    // 2. Executa CTEs (CLASSES, FORNECEDORES, MOLDURA, DADOS_AGRUPADOS) para obter
    //    quantidade por classe de data.
    // 3. Monta grid de período (aWBrowse1) e estrutura de cabeçalhos (_aHdPer).
    // 4. Monta grid mensal (aMensal) usando CTEs MESES, FORNECEDORES_FILTRADOS, DADOS_REAIS.
    // 5. Calcula totals por fornecedor (aTotal) e acumulado (aHdMes).
    // 6. Consulta detalhe de produto/quantidade/valor (aQtdFor, aValFor).
    // 7. Constrói a janela MSDIALOG com três ou quatro grids conforme o tipo
    //    (analítico = 2, gerencial = 1) e botões Exportar/​Firmar.
    // 8. Retorna .T.
Return .T.
```
- Usa variáveis globais `YA7` para obter o ID do forecast, tipo de relatório (`YA7_TPREL`), meses futuros (`YA7_MESFUT`) etc.
- A exportação é disparada por `EXPEXCEL()` através de um botão com ícone `PMSEXCEL.PNG`.

### `fWBrowse*` – Montagem das telas de grid
* Cada função recebe um *header array* (colunas) e um *data array* (linhas) e cria um `LISTBOX` dentro da dialog.
* As colunas são adicionadas dinamicamente via macro (`&("oWBrowseX:AddColumn(...)")`).
* Os arrays são copiados para variáveis globais (`aDados1`, `aDados2`, …) para serem reutilizados na exportação.

### `EXPEXCEL(aCabs, cTipo, oSay)`
```advpl
STATIC FUNCTION EXPEXCEL(aCabs, cTipo, oSay)
    // Cria objeto FWMsExcelEx, preenche worksheets de acordo com cTipo:
    //   - "1"  => Analítico (duas abas: Quantidades e Valores)
    //   - outro => Gerencial (Período, Mensal, Acumulado)
    // Utiliza a estrutura aCabs (cabeçalhos), aDados* (dados dos grids) e
    //   loops para gerar colunas e linhas dinamicamente.
    // Salva arquivo XML em %TEMP%/forecast_<ID>.xml e abre no Excel.
RETURN
```
- O código gera XML, abre-o com `MsExcel():New()` e exibe ao usuário.

### `FIRMAFC()`
```advpl
STATIC FUNCTION FIRMAFC()
    // Verifica se o forecast já está firmado (YA7_FIRME == 'S').
    // Caso não, solicita ao usuário quantos meses firmar (01‑04) usando U_SHPRMBOX.
    // Atualiza campos YA7_FIRME, YA7_MFIRME, YA7_STATUS e desbloqueia registro.
    // Retorna .T. se a operação ocorreu.
RETURN lRet
```
- A operação impede alterações posteriores nos meses firmados.

----

## Fluxo típico de uso
1. **Abrir o relatório** – No browser de Forecast, escolha **Relatório Forecast** (chama `RELFOREC`).
2. **Aguardar a geração** – A barra de progresso “Montando relatório...” aparece via `FwMsgRun`.
3. **Visualizar grids** – São exibidas as abas:
   - **Período** (quantidades por classe de data).
   - **Mensal** (valores por fornecedor/mês).
   - **Acumulado** (total geral).
   - **Fornecedor por Quantidade/Valor** (analítico).
4. **Exportar** – Clique no ícone de Excel; o código chama `EXPEXCEL` e abre o arquivo gerado.
5. **Firmar (analítico apenas)** – Clique no botão **Firmar**; a rotina `FIRMAFC` valida, pede confirmação e marca o forecast como firmado.

----

## Requisitos de ambiente
- Tabelas `YA7`, `YA8`, `SA2`, `SB1`, `PD8`
- Funções auxiliares externas (`U_SHPRMBOX`, `U_FOREMES`, `MsAdvSize`, `TFont`, `FWMsExcelEx`, `MsExcel`, `RetSqlName`, `MpSysOpenQuery`, `StrZero`, `DtOS`, `FirstDate`, `Month`, `Year`, `Add_Months`, `LastDate`, etc.) devem estar disponíveis.
- Ícone `PMSEXCEL.PNG` deve existir no diretório de imagens da aplicação.

----

## Dicas de homologação
1. **Geração de relatório** – Verificar que as três grids aparecem sem erros e que os valores somam aos esperados nas tabelas `YA8`.
2. **Exportação** – Confirmar que o arquivo XML é criado em `%TEMP%/forecast_<ID>.xml` e que o Excel abre com as abas corretas.
3. **Firmar forecast** – Testar a tentativa de firmar um registro já firmado e validar a mensagem: 
   > "O forecast já foi firmado anteriormente. Não é possível realizar alteração".
4. **Persistência** – Verificar no banco que `YA7_FIRME` foi setado para `S`, que `YA7_MFIRME` contém a quantidade de meses selecionados e que `YA7_STATUS` passa a `2`.
5. **Limites** – Experimentar com diferentes valores de `YA7_MESFUT` (meses futuros) e garantir que a lógica de datas (`U_FOREMES`) continue correta ao atravessar o fim de ano.
