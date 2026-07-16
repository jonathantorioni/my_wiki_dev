---
id: shtranesprw
title: SHTRANES.PRW
---

# SHTRANES.PRW

**Transferência automática de produtos com estoque excedente**

### **Dados da Customização**

----

Analista: Jonathan Torioni

----

### **Objetivo**

----

Inclusão de novos parâmetros para controlar a emissão dos pedidos de transferência. Este controle é necessário para a fase de implantação da rotina, permitindo efetuar transferências de produtos, linhas e marcas específicas e sem comprometer a capacidade de vazão da logística.

----

### **Parâmetros**

----

* **ES_TRANMAR** - Marca dos produtos transferência automática
* **ES_TRANLIN** - Linhas dos produtos para transferência automática
* **ES_TRANPRD** - Produtos para transferência automática
* 
Os parâmetros devem ser por filial e criados para todas as filias. Seu conteúdo deve ser inicialmente vazio e quando preenchido, separar os produtos, marcas e linhas com o PIPE.


----

### **Funções**

* **U_SHTRANES(aParam)** - Função responsável por pegar produtos com transferências aprovadas na tabela PCZ e gerar os pedidos para a logistica realizar a separação/envio das mercadorias para o CD
* **EMPPROCESSS()** - Realiza a verificação se a empresa está cadastrada como uma filial a ser abastecida na tabela DB5(Matriz de Abastecimento)
* **GETPRODS()** - Executa a query para pegar todos os produtos que estão aprovados para transferência, deixa o alias aberto dentro da variável private cAlias
* **GERTRANSF()** - Função responsável por gerar as transferências via execauto
* **MONTCAB(cNumPV)** - Função responsável por realizar a montagem do cabeçalho do pedido de transferência
* **U_GETTRANSP(cFilDest)** - Função responsável por retornar a transportadora crossdocking da filial corrente

----
