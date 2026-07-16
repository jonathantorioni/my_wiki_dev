---
id: exprelhtmlprw
title: EXPRELHTML.PRW
---

# EXPRELHTML.PRW

**Exportar relatorios html**

### **Dados da Customização**

----

Analista: Jonathan Torioni

----

### **Especificação da Customização**

----


Está customização tem o objetivo de disponibilizar uma interface para gerar relatórios do tipo planilas em html.

A necessidade surgiu quando nos deparamos que relatórios exportados através da classe **FWMSExcel**, nem sempre podem ser abertos via LibreOffice.


----

### **Especificações da Classe/Method**

----

**expRelhtml** - Classe criada no fonte exprelhtml.prw, responsável por retornar o objeto de montagem do relatório

**Method New(cFile, cPath)** - Method costrutor da classe, responsável por montar e popular as propriedades do objeto. Recebe 2 parâmetros, cFile - indica o nome do arquivo a ser gerado, cPath - indica o diretório onde o arquivo gerado será salvo.

**Method FormHtml()** - Method de uso interno, responsável por montar a estrutura primária do html. **Não deve ser usado por chamadas externas.**

**Method DefTitDoc(cTitle)** - Method responsável por preencher a propriedade do objeto que define o nome/titulo do documento.

**Method AddAba(cTitAba, aHeader, aCols)** - Method responsável por receber e construir as abas/planilhas do relatório em html. Recebe 3 parâmetros, cTitAba - titulo da aba, aHeader - array do tipo lista simples contendo o cabeçalho da tabela, aCols - Array multidimensional contendo cada posição todas as informaçoes da linha/coluna.

**Method Finish()** - Method responsavel por finalizar a estrutura do html.

**Method Save()** - Method responsavel por salvar o arquivo html no path especificado

----

### **Exemplo de relatório**

----

![Funcionamento visual](static/exprelhtml.gif)
