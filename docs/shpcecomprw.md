---
id: shpcecomprw
title: SHPCECOM.PRW
---

# SHPCECOM.PRW

**Cadastro de Fornecedor x Filial Entrega**

**Modulo**: 97 > Atualizações > Compras > Central Compras > Fornec x Fil. Ent

----

## Dados da Customização

**Analista**: Luiz Eduardo

----
## Especificaçãoo da customização

Tem como objetivo cadastrar os fornecedores/loja e para as quais filiais serão entregues.

----

## Especificação de funções e rotinas

- **SHPCECOM** - Função principal;

- **SHVALINC** - Função para validar inclusão - para não cadastrar registro duplicado

- **SHPCEAUX** - Função de importação formato CSV - Fornecedor Auxiliar

- **shpceimp(cFile)** - Função para gravar na tabela PCE - conforme arquivo CSV
