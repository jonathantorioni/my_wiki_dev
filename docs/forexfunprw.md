---
id: forexfunprw
title: FOREXFUN.PRW
---

# FOREXFUN.PRW

**Analista:** Jonathan Everton Torioni Oliveira

---

## Visão geral

Este módulo contém funções auxiliares para o programa de **Forecast**. Ele fornece:

- Uma interface visual com caixa de seleção de dias do mês (`FRETDCF`).
- Funções de cálculo de períodos futuros (`FOREMES`).
- Funções de apoio para obter os dias selecionados (`YA7DCOM`).

A interface permite ao usuário escolher até quatro dias de compra futura, retornando a lista como uma string separada por ponto‑e‑vírgula. As funções de cálculo geram arrays de datas a serem usadas em relatórios de forecast.

----

## Funções principais

| Função | Tipo | Descrição |
|--------|------|-----------|
| `FRETDCF()` | **User Function** | Cria um diálogo com 31 checkboxes (um para cada dia do mês) para selecionar dias de compra futura. Limita a seleção a no máximo 4 dias e devolve a string concatenada em `cYA7DCOM`. |
| `FRETDIAS()` | **Static Function** | Monta o array `aCheck` contendo o estado de cada checkbox e os valores dos dias selecionados. |
| `YA7DCOM()` | **User Function** | Retorna o valor de `cYA7DCOM` – a string de dias selecionados – para uso em consultas. |
| `FOREMES(aPeriodo, lCab, nMesFutPer)` | **User Function** | Recebe um array de dias (`aPeriodo`) e gera um array de datas para os próximos `nMesFutPer` meses. Opcionalmente inclui o cabeçalho "Fornecedor" quando `lCab` é verdadeiro. |


### `FRETDCF()`
```advpl
USER FUNCTION FRETDCF()
    // Declara variáveis de controle dos 31 checkboxes
    // Cria o diálogo (180,180) → (350,400)
    // Cada checkbox tem um prompt com o número do dia
    // Botão "Salvar" chama FRETDIAS() e fecha o diálogo
    // Após o diálogo, percorre aCheck e concatena até 4 dias selecionados
    // Armazena o resultado em cYA7DCOM
RETURN .T.
```
- O diálogo usa `DEFINE DIALOG` e `CheckBox` widgets.
- O botão salva a seleção e fecha a janela.

### `FRETDIAS()`
```advpl
STATIC FUNCTION FRETDIAS()
    aCheck := {}
    // Cada linha adiciona o estado (verdadeiro/falso) e o dia formatado
    Aadd(aCheck, {lCheck1 , StrZero( Val(SubStr(oCheck1 :cTitle,1,2)), 2 )})
    // ... Repete até lCheck31
    RETURN
```
- Constrói `aCheck` usado por `FRETDCF()` para gerar a string final.

### `YA7DCOM()`
```advpl
USER FUNCTION YA7DCOM()
    RETURN cYA7DCOM
```
- Função simples que devolve a string de dias selecionados.

### `FOREMES()`
```advpl
User Function FOREMES(aPeriodo,lCab,nMesFutPer)
    // aPeriodo – array de strings com dias selecionados (ex.: {"01","15","30"})
    // nMesFutPer – número de meses futuros a projetar
    // lCab – inclui cabeçalho "Fornecedor" quando .T.
    // Gera datas válidas a partir da data atual, avançando mês a mês
    // Tratamento de dias inexistentes (ex.: 30/02) usando `Empty(dData)`
    // Retorna array de datas ou cabeçalhos formatados
RETURN aDatas
```
- Utiliza `StoD`, `StrZero`, `Month`, `Year` e `LastDate` para garantir datas válidas.

## Fluxo típico de uso
1. **Abrir a seleção de dias** – Chame `FRETDCF()` a partir da tela de consulta padrão.
2. **Selecionar até quatro dias** – Marque os checkboxes desejados e clique em *Salvar*.
3. **Obter a string de dias** – A função `YA7DCOM()` devolve algo como `"01;15;30;"`.
4. **Gerar períodos** – Passe o array resultante (`aPeriodo`) para `FOREMES()` junto com a quantidade de meses futuros.
5. **Utilizar nas rotinas de forecast** – Os valores são consumidos por outras rotinas (ex.: relatórios, geração de forecasts).

---

## Requisitos de ambiente
- Tabela `YA7`
- Funções auxiliares externas (`StrZero`, `StoD`, `Date`, etc.) devem estar disponíveis no ambiente.

---

## Dicas de homologação
1. Verificar que o diálogo exibe 31 checkboxes corretamente.
2. Confirmar que ao selecionar mais de quatro dias a rotina exibe o alerta "Total selecionado é maior que 4 dias".
3. Testar a geração de datas em `FOREMES` para diferentes valores de `nMesFutPer`, especialmente ao cruzar o fim do ano.
4. Validar que `YA7DCOM()` devolve a string esperada e que ela pode ser usada em filtros de consulta padrão.
