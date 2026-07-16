---
id: relratseprw
title: RELRATSE.PRW
---

# RELRATSE.PRW

**Relatório Integração Serasa**

----

## Dados da Customização

Analista: Carlos Henrique Mendes da Silva

----
## Especificaçãoo da customização

Tem como objetivo realizar um relatório contendo todas as movimentações com a Integração Serasa.

----

## Especificação de funções e rotinas

**RELRATSE** - Função principal que contém a tela de parametros para determinar o filtro utilizado na geração do relatorio. 

**PRCQUERY** - Realiza a consulta na tabela ZN9 e armazena as informações dentro de um array.

Parametros: 

- cDataIni - Data inical 

- cDataFim - Data final

- cEmpDe  - Empresa de

- cEmpAte - Empresa ate

- cFilDe  - Filial de

- cFilAte - Filial ate


**GERACSV** - Função resposável por gerar o arquivo CSV de acordo com as regras estabelecidas. 

Regras:

- Se o cliente for do Tipo Fisico, o valor deve ser atribuido a coluna: *VALOR PF*;
- Se o cliente for do tipo Juridico e possuir quadro social, o valor deve ser atribuido a coluna: *VALOR PJ COM QUADRO SOCIAL*;
- Se o cliente for do tipo Juridico e não possuir quadro social, o valor deve ser atribuido a coluna: *VALOR PJ SEM QUADRO SOCIAL*;


----

## Especificação de parametros

- **Tabela SXB** - Consulta padrão
- **XB_ALIAS** - ATVEMP, ATVFIL
