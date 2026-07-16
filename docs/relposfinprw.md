---
id: relposfinprw
title: RELPOSFIN.PRW
---

# RELPOSFIN.PRW

**Relatório Consolidado Fluxo Financeiro**

----

## Dados da Customização

Analista: Carlos Henrique

----
## Especificaçãoo da customização

Tem como objetivo realizar um relatório consolidado de toda empresa conforme periodo com os dados de Recebidos, Pagos, Saldo e Acumulado.

----

## Especificação de funções e rotinas

- **RELPOFIN** - Funçãoo principal que determina se a execuçãoo está sendo via JOB ou tela;

- **VIEWRELFLUXO** - Cria uma tela para informar o periodo;

- **VALCAMPOS** - Validaçãoo dos campos digitados;

- **POSFIN** - Funçãoo responsável por processar todas as empresas gravando os Realizados em um array e somando todos os dias no final da operação;

- **REALIZADOS** - Funçãoo que processa da Query na tabela SE5 com o realizado Pagos e Recebidos da empresa;

- **SENDEMAIL** - Funçãoo de passagem de parametros para o envio de e-mail;

- **MANDAEMAIL** - Função para disparar o e-mail com os anexos;

- **GETEMPRESA** - Função para validar as empresas ativas;

- **GERACSV** - Função responsável pela criação da planilha em CSV;

- **INIJOBREL** - Função para iniciar o ambiente com o RPCSETENV();

- **GETCQERY** - Funçãoo responsável por trazer a tabela utilizada em cada empresa, substituindo a função RetSqlName() para corrigir erro de processamento;

----

## Especificação de parametros

- **ES_FLXFIN** - E-mails que receberão o relatório consolidado.
