---
id: telfatautoprw
title: TELFATAUTO.PRW
---

# TELFATAUTO.PRW

**Faturamento automático**

### **Dados da Customização**

----

Analista: Jonathan Torioni

----

### **Especificação da Customização**

----


Está customização tem o objetivo de realizar o faturamento e transmissão dos documentos.

Inicialmente está customização foi desenvolvida para melhorar o antigo faturamento automático (transdanfe.prw).


----

### **Especificações das Funções e Rotinas**

----

**U_TELFATAUTO(nOpcA)** - Função responsável por orquestrar a montagem da tela de faturamento automático e iniciar as threads responsáveis por faturar e transmitir.

**fBrwP(lAtuTela)** - Função responsável montar o grid dos pedidos. 

**fBrwN(lAtuTela)** - Função responsável montar o grid das notas.

**Transmite(lMonta)** - Função responsável retornar dados do grid de notas e também por transmitir as notas.

**Fatura(lMonta)** - Função responsável retornar dados do grid dos pedidos e também por faturar as notas

**fChekbug(e,cFil,cPed,cArq)** - Função responsável tratamento de erros legados do transdanfe.

**fAtuTela()** - Função orquestrar a atualização dos grids.

**fAtuMtr()** - Função atualiar o objeto TMeter

**U_SHSPEDTR(cSerie,cNotaIni,cNotaFim,lCTe,lRetorno,aNotas)** - Rotina de remessa da Nota fiscal eletronica, Service SPED - utilizada em personalizacoes.

**IsReady(cURL,nTipo,lHelp,lUsaColab)** - Rotina de verificação se o TSS esta apto para transmissões.

**U_SendTran(aParam,_cMod,_cOpcVer,_cUser)** - Funçao responsável por orquestrar a abertura e fechamento da thread de transmissao de notas.

**U_SendFat(aParam,_cMod,_cOpcVer,_cUser)** - Funçao responsável por orquestrar a abertura e fechamento da thread de faturamento dos pedidos.

**GetIdEnt(lUsaColab)** - Funçao responsavel por retornar o idEnt da empresa/filial no TSS.

**Imprime(_cSem1Danf,_cModImp)** - Impressao Automatica de Danfe - COPIA melhorada do transdanfe.

**XPERG()** - Função de compatibilizacao.

**SpedDanfe()** - Impressao da danfe de acordo com o setup inicial.

**RETLEG(cConteud, xAdic)** - Função responsável por retornar as diversas legendas no grid de notas.

----

### **Parâmetros**

----

**ES_NEWFAT** - Caso não seja criado, assume o valor .T., garantindo que a nova rotina será executada após a implantação. Caso seja preenchida com .F., todo o processo voltará a ser executado como antes, porém com as melhorias de performance feitas nas querys relacionadas.
