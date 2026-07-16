---
id: skalephapiprw
title: SKALEPHAPI.PRW
---

# SKALEPHAPI.PRW

----

## **Dados da Customização**

**Analista:** Jose Antonio  
**Data:** 07/07/2023    
**Versão:** 1.0 

----

## Especificação da Customização

Produtos GM que será comercializado pelo Mercado livre.

----

## Especificação das Funções e Rotinas

----

### SCHPRDML(ES_GMFILSK)

**Descrição**   
Schedulle para dar Start função SKMLPRDG responsavel obter os produtos GM que será comercializado pelo Mercado livre.

**Parâmetros**

Nome|Tipo|Uso|Descrição
-|-|-|-
ES_GMFILSK|character|Obrigatório|Filiais que irá atuar Mercado Livre. No momento somete a filial 42-Vila Maria ira atuar.

**Retorno**


Tipo|Descrição
-|-
variant|return_description

----

### SKMLPRDG 

**Descrição**   
Esta função faz uma solicitação GET para obter os produtos da Alephee/Mercado Livre para atualizar preço e estoque.
Após obter os produto e aplicar preço conforme a politica da SK é feito uma solicitação POST na API da Alephee para atualizar os preços e estoques.

**Parâmetros**

Nome|Tipo|Uso|Descrição
-|-|-|-
ES_URLALEP|character|Obrigatório|A URL da API da Alephee
ES_ACCALEP|character|Obrigatório|O ID da conta da Alephee
ES_KEYALEP|character|Obrigatório|A chave da API da Alephee
ES_LIMALEP|character|Obrigatório|O número máximo de produtos a serem retornados na solicitação GET
ES_OFIALEP|character|Obrigatório|O deslocamento (offset) para determinar o ponto de partida dos resultados da solicitação GET

**Retorno**

Tipo|Descrição
-|-
lRet|Indicando se a atualização de preços e estoques foi bem-sucedida ou não.

----

### PRICEML

**Descrição**  
Preço aplicado no Mercado Livre/Aleephe

**Parâmetros**

Nome|Tipo|Uso|Descrição
-|-|-|-
cProd|character|Obrigatório|Codigo Produto GM
cUf|character|Obrigatório|Estados
cFil|character|Obrigatório|Filial vendedora no Mercado Livre

**Retorno**

Tipo|Descrição
-|-
oJson|objeto contendo npreços aplicvado para o mercado livre

----

### RETORNAUFSK

**Descrição**   
Funçao responsável por retornar todas as uf que a SK atual.

**Retorno**

Tipo|Descrição
-|-
aRet|Estados que SK atua
