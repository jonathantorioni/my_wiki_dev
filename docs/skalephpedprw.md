---
id: skalephpedprw
title: SKALEPHPED.PRW
---

# SKALEPHPED.PRW

----

## **Dados da Customização**

**Analista:** Jose Antonio  
**Data:** 07/07/2023    
**Versão:** 1.0 

----

## Especificação da Customização

Faz get na plataforma Mercado Livre/Aleephee dos pedidos com produtos GM

----

## Especificação das Funções e Rotinas

----

### SKPEDGETA

**Descrição**   
Faz get na plataforma Mercado Livre/Aleephee dos pedidos com produtos GM.

**Retorno**

Tipo|Descrição
-|-
variant|return_description

----

### GravaPedido

**Descrição**   
Prepara pedido vindo do Mercado Livre para incluir na SK.

**Parâmetros**

Nome|Tipo|Uso|Descrição
-|-|-|-
aPedido|array|Obrigatório|Contendo os pedidos disponiveis no Mercado Livre


**Retorno**

Tipo|Descrição
-|-
aRet|Retorna array com pedidosinformação do pedido gerado

----

### GETPEDIDOML 

**Descrição**  
Obten lista de pedido gerado na plataforma do Mercado Livre para serem faturados na SK

**Parâmetros**

Nome|Tipo|Uso|Descrição
-|-|-|-
ES_URLALEP|character|Obrigatório|A URL da API da Alephee
ES_ACCALEP|character|Obrigatório|O ID da conta da Alephee
ES_KEYALEP|character|Obrigatório|A chave da API da Alephee
ES_SDATEFR|character|Obrigatório|Obter a partir de uma data

**Retorno**

Tipo|Descrição
-|-
oPed["Results"]|Objeto com o resultado do pedido

----

### PUTPEDIDOML

**Descrição**   
Faz um PUT dos pedido gerado na SK na plataforma do Mercado Livre.

**Parâmetros**

Nome|Tipo|Uso|Descrição
-|-|-|-
cJson|character|Obrigatório|contendo informação do pedido gerado no Protheus

**Retorno**

Tipo|Descrição
-|-
variant|return_description

----

### IncPedidoVenda

**Descrição**   
Inclui pedidos que vem da paltaforma do Mercado Livre/Aleephe para SK

**Parâmetros**

Nome|Tipo|Uso|Descrição
-|-|-|-
cJson|character|Obrigatório|contendo informações do pedido

**Retorno**

Tipo|Descrição
-|-
aParam|array, contendo informações do Pedido
