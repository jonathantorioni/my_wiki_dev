---
id: mx181prw
title: MX181.PRW
---

# MX181.PRW

**MX181.PRW - Modelo de dados para efetivação da sugestão de compras**

----

### Dados da customização

Analista responsável: Jonathan Torioni

----

### Especificação da customização

Este fonte foi copiado do mata181 padrão e foi modificado para atender as necessidades estabelecidas no projeto do central de compras.
A falta de pontos de entrada na rotina não possibilitava atender customizações essenciais para o projeto.

----

### Especificação de tabelas e índices

Não possui

-----

### Especificação de parâmetros

Não possui

----

### Especificação das funções

* **ModelDef** - Definicao do Modelo de dados

* **A181ActMod** - Realiza soma do preco e alimenta o campo DBH_VALTOT

* **A181VLMod** - Validacao do modelo para nao permitir alterar registros efetivados

* **X181Efet** - Instancia os modelos de dados da rotina Central de Compras - DBJ/DBH/DBI

* **A181GerDoc** - Efetiva as necessidades e cria os documentos(Solicitacao, Pedido de Compra ou Pedido de Venda)

* **A181PedVend** - Função responsável por fazer a geração do pedido de vendas (Leonardo Quintana)

* **X181GerPV** - Função responsável por fazer a geração do pedido de vendas (Leonardo Quintana)

* **ViewDef** - Função criada para atender o modelo MVC da totvs, efetuando a montagem das telas

* **U_MX181FAT** - Calculo de quantidade faturada (Leonardo Quintana)

* **U_SHRETVAL** - Função responsável por preencher os campos virtuais na tabeja DBJ Quantidade e Valor total

:::info

Este fonte foi copiado do fonte padrão mata181, por este motivo se faz necessário a utilização dos includes mata181.ch

:::
