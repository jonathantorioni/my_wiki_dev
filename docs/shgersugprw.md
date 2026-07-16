---
id: shgersugprw
title: SHGERSUG.PRW
---

# SHGERSUG.PRW

**Geração sugestão de compras automaticas**

### **Dados da Customização**

----

**Analista**: Jonathan Torioni

----

### **Especificação da Customização**

----


Está customização tem o objetivo de realizar a geração da sugestão de compras automaticamente.

Solicitado na fase 2 do projeto central de compras, pelo sr Mauricio Gouveia, com intenção de evitar que os compradores tenham que gerar manualmente as sugestões de compras.

Esta customização foi projetada para rodar via schedule, necessitando configurar apenas uma tarefa por empresa.


----

### **Especificações das Funções e Rotinas**

----

- **u_SHGERSUG(aParam)** - Função responsável por ser chamada no schedule, buscar os produtos para gerar sugestões de compras automaticamente, busca os produtos e já classifica como aprovados ou reprovados.(**aParam - Array de contendo \{cEmpAnt, cFilAnt\}**);

- **SHSEPARA(aDados)** - aDados - Array multi dimencial, cada posição do array representa uma classe (b1_xclasse) e para cada posição existem os arrays de cada produto referente a classe.(**aDados - Array multi dimencial, cada posição do array representa uma classe (b1_xclasse) e para cada posição existem os arrays de cada produto referente a classe.**)

- **PrevCons(cFilAba,cProduto,cFornece,cLoja, cEmp, lProteg)** - Função responsável por retornar a previsao de consumo com base nos parametros informados.(
**cFilAba - Filial que está sendo abastecida**
**cProduto - Produto para calculo**
**cFornece - Fornecedor do produto**
**cLoja - Loja do fornecedor**
**cEmp - Empresa de busca**
**lProteg - Tratativa para estoque protegido**
**aEstat - Dados estatisticos do produto**)

- **AGGETCUR(_cFil, _cCod, cEmp)** - Função responsável por retornar as curvas de hit e valor do produto na filial

- **RFILDIS** - Função responsável por retornar as filiais distribuidoras

- **RFILABA(cFilDis)** - Função responsável por retornar as filiais distribuidoras

- **GERSUG(aParam)** - Função responsável por fazer a montagem das sugestões de compras

- **MONTASUG(aDados)** - Função responsável por fazer o envio da montagem das sugestões, aprovadas e não aprovadas(**aDados - array de dados com as informações que vão gerar a sugestão de compras**)

- **GTCMXCLA(cClasse)** - Função responsável por pegar o comprador referente a classe dos produtos.

- **GERREL(aAprov, aReprov)** - Função responsável por gerar o relatório de sugestoes aprovadas e não aprovadas

- **DELSUG()** - Função responsável por deletar as sugestoes de compras que nao foram analisadas legenda azul. Executa uma vez a cada processamento.

- **SENDNOTF()** - Função responsável por montar o corpo do e-mail e realizar o envio do e-mail

- **RETTOT(cSugest)** - Função responsável por retornar total da sugestao.

- **CTNCURH(cSug)** - Função responsável por retornar total de itens por curva de hit na sugestao.
