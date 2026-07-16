---
id: shtranfprw
title: SHTRANF.PRW
---

# SHTRANF.PRW

**Desabastecimento automatico de produtos sem giro**

### Dados da customização

Analista responsável: Jonathan Torioni

----

### Especificação da customização

Esta customização tem como objetivo realizar o desabastecimento automático de mercadoria nas filiais abastecidas por um CD.
Toda a mercadoria com mais de 12 meses que não tenha compras e nem vendas, são consideradas sem giro.

----

### Especificação de funções e rotinas

* **U_SHTRANF** - Função responsável por remover os produtos que não possui demanda a mais de um ano.

* **EMPPROCESS** - Realiza a verificacao se a empresa esta cadastrada como uma distribuidora na tabela DB5(Matriz de Abastecimento).

* **GETPRODS** - Executa a query para pegar todos os produtos que estao aprovados para transferir e deixa o alias aberto dentro da variavel Private cAlias Alias de pesquisa PCZ

* **GERTRANSF** - Funcao responsavel por gerar as transferencias via execauto

----

### Especificação de parametros

* **ES_COMPAUT** - Código do usuário de compras automático

:::info
Este fonte foi desenvolvido para rodar apenas via schedule.
:::
