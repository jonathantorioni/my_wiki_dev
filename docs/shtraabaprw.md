---
id: shtraabaprw
title: SHTRAABA.PRW
---

# SHTRAABA.PRW

**Abastecimento automático das filiais atendidas por um CD**

### Dados da customização

Analista responsável: Jonathan Torioni

----

### Especificação da customização

Esta customização tem como objetivo realizar o abastecimento automático de mercadoria nas filiais abastecidas por um CD.
Todo o calculo é realizado com analise estatistica das vendas de cada mercadoria, com base nisto, é definida a quantidade de cada mercadoria que será enviada para a filial.

----

### Especificação de funções e rotinas

* **U_SHTRAABA** - Funcao responsavel por pegar produtos com transferencias abastecimento aprovadas na tabela PCZ e gerar os pedidos de transferencias para a logistica realizar a separacao/envio das mercadorias para a filial a ser abastecida.
Este fonte será configurado no schedule com a recursividade 7 dias

* **EMPPROCESS** - Realiza a verificacao se a empresa esta cadastrada como uma distribuidora na tabela DB5(Matriz de Abastecimento).

* **GETPRODS** - Executa a query para pegar todos os produtos que estao aprovados para transferir e deixa o alias aberto dentro da variavel Private cAlias Alias de pesquisa PCZ

* **GERTRANSF** - Função responsavel por gerar as transferencias via execauto

* **MONTCAB** - Função realizar a montagem do cabecalho do pedido de transferencia

* **CALNECEST** - Função responsavel por calcular a necessidade bruta da distribuidora e verificar a quantidade possivel para transferir para a filial abastecida

* **SENDNOTF** - Função responsavel por notificar os compradores da necessidade de abastecimento

* **EFETPV** - Função responsavel gravar o pedido de transferência

* **GETMAIL** - Função responsavel os e-mails dos compradores

* **GERNTRAN(cFilDest, aDados)** - Função responsável por gerar o arquivo de consolidacao dos produtos que não foram realizados abastecimentos

* **GETTT(_cFil, cProd)** - Função responsável por pegar a soma do produto que está em transito transferencia, notas de transferencias que ja foram faturadas, porém que ainda não deram entrada no estoque da filial.

* **GTDISTR(_cFil)** - Função responsável por retornar a filial distribuidora referente a filial informada.

* **AGGETCUR(_cFil, _cCod, cEmp)** - Função responsável por retornar as curvaas de git e valor do produto da filial informada.

* **RETDIAS(cFilDest, cProd)** - Função responsável por retornar os dias de abastecimento da filial sendo abastecida.

----

### Especificação de parametros

* **ES_COMPAUT** - Código do usuário de compras automático
* **ES_TRANMAR** - Parâmetro para realizar o abastecimento somente de marcas/fornecedor (B1_XCLASSE), informar o código e separar por pipe (|) caso haja mais de um.
* **ES_TRANPRD** - Parâmetro para realizar o abastecimento somente do produto específico, informar o código do produto no parâmetro e separar por pipe (|)
* **ES_PEDTRAN** - Parêmetro utilizado nos pedidos de vendas para delimitar a quantidade maxima de itens dentro do pedido de vendas, valor default 20.




:::info
Este fonte foi desenvolvido para rodar apenas via schedule.
:::
