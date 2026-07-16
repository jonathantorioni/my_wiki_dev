---
id: shsugtranprw
title: SHSUGTRAN.PRW
---

# SHSUGTRAN.PRW

**Alimentador de tabela temporaria PCZ para transferências automatizadas**

### Dados da customização

Analista responsável: Jonathan Torioni

----

### Especificação da customização

O objetivo deste programa é pegar todos os produtos que tiveram calculos estatisticos e
verificar se o estoque está excedente ou com falta, gravar esses dados em uma tabela de sugestão de transferência.
Os dados para formação de estoque serão com base na parâmetrização dos da tabela PCX

----

### Especificação de funções e rotinas

* **U_SHSUGTRAN** - Função responsável orquestradora para ser executada em schedule

* **GETPROD** - Função responsável por retornar o Alias com os produtos a serem revisados

* **SHSUGTRAN** - Função responsável por gerar uma tabela temporária com os dados de todos os produtos com estoque excedido ou com a necessidade de abastecimento da filial

* **CALNECEST** - Função responsável por calcular a necessidade bruta da filial

* **DELANTI** - Função responsável deletar todos registro que não tiveram analise no dia corrente

* **GetTT** - Função responsável por pegar a soma do porduto que está em transito transferência notas de transferencias que já foram faturadas, porem ainda não deram entrada no estoque da filial.

* **GtDistr** - Função responsável retornar a filial distribuidora da filial passada

* **GtSemDem** - Função responsável pegar e gravar na tavela PCZ todos os produtos da filial que não possuem demanda a mais de 12 meses

* **U_SHRDIEN** - Função responsável por retornar a quantidade de dias desde a ultima entrada do produto na filial

* **Class EXCECAOPRODUTOSTRANSF** - Classe responsável por fazer as cargas e verificar excecoes de produtos para transferencias

* **Method New Class EXCECAOPRODUTOSTRANSF** - Contrutor da classe EXCECAOPRODUTOSTRANSF

* **Method CargaVar Class EXCECAOPRODUTOSTRANSF** - Carrega as propriedades do objeto com os produtos, marcas, grupos da excecao

* **Method Excecao Class EXCECAOPRODUTOSTRANSF** - Verifica se o produto é uma excecao cadastrada

----

### Especificação de parametros

* **ES_TRANDIA** - Quantidades de dias para gerar analise est excedente
* **ES_ABASTEC** - Quantidade de dias para abastecimento                        
* **ES_NREG1** - Quantidade de dias para atendimento da regra 1, solicitada pelo Wagner das Neves Iwashita
* **ES_NREG2** - Quantidade de dias para atendimento da regra 2, solicitada pelo Wagner das Neves Iwashita
* **ES_NREG4** - Quantidade de dias para atendimento da regra 4, solicitada pelo Wagner das Neves Iwashita
* **ES_NREG5** - Quantidade de dias para atendimento da regra 5, solicitada pelo Wagner das Neves Iwashita

:::info
As regras solicitadas pelo **Wagner das Neves Iwashita** estão em um e-mail com as primeira solicitações que ele realizou ao entrar na empresa SK, caso haja duvidas, o e-mail deve ser consultado para maior entendimento.
:::

----

### Operações de transferências

Detalhamento de cada operação que pode ser encontrado no campo PCZ_OPER da tabela PCZ

1. Abastecimento da filial conforme necessidade
2. **Desabastecimento da filial conforme regra:** Entrada > 90 dias e menor que 120 dias, com média de vendas diferente de zero. Fórmula para cálculo da transferência:    Estoque - (Média do período x 60 dias) = quantidade a ser transferida para CD.
3. **Desabastecimento da filial conforme regra:** Produto sem demanda +12meses
4. **Desabastecimento da filial conforme regra:** Entrada > 90 e menor que 120 e Média de vendas = 0. Estoque Filial x 50%
5. **Desabastecimento da filial conforme regra:** Entrada > 120 e menor que 180 e Média de vendas = 0. Estoque Filial x 80%
6. **Desabastecimento da filial conforme regra:** Entrada > 120 e menor que 180 e Média de vendas = 0. Estoque Filial x 80%

:::info
Este fonte foi desenvolvido para rodar apenas via schedule.
:::
