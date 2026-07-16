---
id: mprcautorradprw
title: MPRCAUTORRAD.PRW
---

# MPRCAUTORRAD.PRW

**MPRCAUTORRAD.PRW - Formação de preços Autorrad**

## **Dados da Customização**

**Analista:** Jonathan Everton Torioni Oliveira

----

## **Especificação da Customização**


Schedule de atualização dos preços, para executar sempre no período noturno, rodar todos os dias.

Os preços serão formados sempre pelo ultimo custo de entrada da mercadoria e com base no MKP cadastrado na rotina Cadastro de MJP(CADMKP.PRW).

Os preços devem ser formados e atualizados somente na filial 54 - Autorrad.

----

## **Especificação de Perguntes**

Não houve modificações em perguntes.

----

## **Especificação das Funções e Rotinas**

**[U_MPRCAUTO]** - Funcao responsavel por realalizar atualizacao de preços da autorrad.


**[GETPRDC]** - Funcao responsavel por pegar os produtos que vão atualizar os preços.

----

## **Critérios para Validação**

1. Selecionar um produto e buscar o ultimo custo unitário de entrada da mercadoria na SD1.
2. Rodar a rotina via schedule ou pela chamada do SmartClient.
3. Verificar se o preço formado na B0_PRV1 é igual ao ultimo custo, multiplicado pele MKP cadastrado pelo produto.
