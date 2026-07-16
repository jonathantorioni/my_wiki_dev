---
id: shextcomprw
title: SHEXTCOM.PRW
---

# SHEXTCOM.PRW

**Relatório da sugestão de compras**

### Dados da customização

Analista responsável: Jonathan Torioni

----

### Especificação da customização

Esta customização foi desenvolvida para atender uma necessidade do departamento de compras em ter um relatório com todas as informações necessárias dos produtos gerados na sugestão de compras.
As informações contidas nesse relatório é um concenso entre os compradores da empresa SK.
Toda e qualquer alteração nesta customização deve passar por uma autorização geral do departamento.

----

### Especificação das funções e rotinas

* **U_SHEXTCOM** - Função responsável por gerar um arquivo XML(Excel) para visuazação da sugestão de compras Prod/Filial

* **U_SHRETTRA** - Função responsável por buscar e retornar a quantidade de um determinado produto que ainda está em transito

* **U_SHRETTRA** - Função responsável por buscar o texto contendo a aplicação do produto recebido

* **U_GETCURVA** - Função responsável por buscar a curva que o produto está classificado

* **U_GETULCOMP** - Função responsável por buscar os dados da ultima compra do produto recebido

* **U_GETCATEGO** - Função responsável por retornar o texto completo da categoria

* **U_GTVENDAS** - Função responsável por pegar a quantidade total de itens vendidos em um terminado período de um determinado produto

* **U_GETBOTRANS** - Função responsável por retornar a quantidade total de BO em transito

* **U_GETMEDPER** - Função responsável por retornar a média analisada no período

* **RETTITD** - Função responsável por retornar o titulo da data informada para o relatório

:::info
Este relatório só pode ser acessado através da rotina central de compras (**xmata179x.prw**)
:::
