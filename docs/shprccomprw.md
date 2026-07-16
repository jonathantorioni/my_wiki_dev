---
id: shprccom
title: SHPRCCOM.PRW
---

# SHPRCCOM.PRW

**Coleta estatística pra sugestão de compras**

### **Dados da Customização**

----

Analista: Jonathan Torioni

----

### **Objetivo**

----

Efetuar os cálculos de curva estatística para obter valores de média de vendas, variância, desvio padrão e fator multiplicador de Z.

----

### **Especificações das Funções e Rotinas**

----

**SHPRCCOM(aParam)** -  Função principal, que será cadastrada no schedule.

**GetProd()** – Função responsável por retornar a lista de produtos que serão processados. Retorna um vetor com os produtos.

**GetAmost(aArray)** – Função responsável por receber um array com os produtos e devolver uma amostra ajustada, removendo picos e vales e/ou acrescenta os dias que não tiveram vendas.

**GRVTEMP(aArray)** -  Grava os dados do vetor recebido em uma tabela temporária.

**GERADADOS()** – Grava os dados já calculados de média, desvio padrão, variância, quantidade de amostras na tabela DESVXPROD.


----

### **Regra de processamento**

----

A Função U_SHPRCCOM está projetada para ter sua chamada via schedule e efetuar os devidos processamento conforme cadastramento realizado na tabela PCX.

A função tem como base os campos: 

* PCX_ANOANT - QUANTIDADE DE DIAS DE ANALISE ANO ANTERIOR 	- NUMERICO 3
* PCX_ANOATU - QUANTIDADE DE DIAS DE ANALISE ANO ATUAL	- NUMERICO 3
* PCX_EXCPIC - EXCLUSAO DE PICOS				- NUMERICO 3
* PCX_EXCVAL - EXCLUSAO DE VALES				- NUMERICO 3
* PCX_EXCZER - EXCLUSAO DIAS ZERADOS			- CHAR 1
* 
Os campos descritos acima, influenciam diretamente nos cálculos.
