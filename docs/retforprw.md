---
id: retforprw
title: RETFOR.PRW
---

# RETFOR.PRW

**Analista:** Jonathan Everton Torioni Oliveira

----

## Visão geral

Esta rotina fornece uma **consulta F3 personalizada** para a seleção de fornecedores dentro do módulo de Forecast. Ela exibe uma lista de fornecedores ativos (não bloqueados) em uma tela de seleção múltipla, permite ao usuário marcar até um número limitado de itens e devolve os códigos selecionados como uma string separada por vírgulas.

----

## Funções principais

| Função | Tipo | Descrição |
|--------|------|-----------|
| `retfor()` | **User Function** | Monta a query de fornecedores, exibe a tela de seleção (`f_Opcoes`) e retorna a string de códigos selecionados (`cRetF3`). |

### `retfor()`
```advpl
User Function retfor()
    Local cAlias      := ""
    Local cReadVar    := ReadVar()          // variável de origem que receberá o retorno
    Local aRet        := {}                // códigos selecionados (com descrição)
    Local aFornSel    := {}                // lista exibida na tela (código - nome)
    Local cRetF3      := ""
    Local cTitulo     := "Fornecedores"
    Local cQry        := ""
    Local nx          := 0
    Local cCodFor     := ""
    Local cNomeFor    := ""
    Local nMax        := IIF(MV_PAR01 == "1", 1, 4) // limite de seleções

    // ==== Monta a query ==== 
    cQry += " SELECT DISTINCT(A2_COD), A2_NOME " + CRLF
    cQry += " FROM " + RetSqlName("SA2") + " SA2 " + CRLF
    cQry += " WHERE D_E_L_E_T_ = ' ' " + CRLF
    cQry += " AND A2_FILIAL = '  ' " + CRLF
    cQry += " AND A2_MSBLQL <> '1' " + CRLF
    cQry += " ORDER BY A2_COD " + CRLF

    cAlias := MpSysOpenQuery(cQry)

    // ==== Preenche array para a tela ==== 
    While (cAlias)->(!EOF())
        cCodFor  := AllTrim((cAlias)->A2_COD)
        cNomeFor := AllTrim((cAlias)->A2_NOME)
        AAdd(aFornSel, cCodFor + " - " + cNomeFor)
        (cAlias)->(DbSkip())
    EndDo
    (cAlias)->(DbCloseArea())

    // ==== Exibe tela multiseleção ==== 
    // f_Opcoes(@aRet, título, @aFornSel, …, nMax, .T.)
    If f_Opcoes(@aRet, cTitulo, @aFornSel, "", NIL, NIL, .F., 6, nMax, .T.)
        // Monta retorno apenas com o código do fornecedor
        For nx := 1 To Len(aRet)
            cRetF3 += AllTrim(SubStr(aRet[nx], 1, At("-", aRet[nx]) - 1)) + ","
        Next
        // grava no campo de origem e atualiza tela
        SetMemVar(cReadVar, cRetF3)
        SysRefresh(.T.)
    EndIf

    Return cRetF3
```
- **Limite de seleção** (`nMax`) é determinado pelo parâmetro de tela `MV_PAR01`: 1 fornecedor quando `MV_PAR01 == "1"`, caso contrário até 4.
- A string retornada tem a forma `"COD1,COD2,"` (vírgula final incluída).

----

## Fluxo típico de uso
1. **Acionar a consulta F3** – Em uma tela que requer fornecedor, o usuário pressiona `F3` que chama `retfor()`.
2. **Selecionar fornecedores** – A janela exibida lista `cCodFor - cNomeFor` para cada fornecedor ativo.
3. **Confirmar** – Ao confirmar, a função devolve os códigos selecionados em `cRetF3` e preenche o campo de origem.
4. **Processamento posterior** – O código de fornecedores pode ser usado em consultas de forecast, geração de relatórios, etc.

----

## Requisitos de ambiente
- Tabela **SA2** (fornecedores).
- Funções auxiliares externas (`ReadVar`, `SetMemVar`, `SysRefresh`, `RetSqlName`, `MpSysOpenQuery`, `f_Opcoes`, `CRLF`) devem estar disponíveis no ambiente.
- Variável de parâmetro `MV_PAR01` deve ser configurada previamente para controlar o limite de seleção.

----

## Dicas de homologação
1. **Seleção simples** – Verificar que ao selecionar um único fornecedor a string retornada contém apenas o código correspondente.
2. **Seleção múltipla** – Quando `MV_PAR01 <> "1"`, tentar selecionar até 4 fornecedores e confirmar que a string contém todos os códigos separados por vírgula.
3. **Limite de itens** – Definir `MV_PAR01 = "1"` e tentar selecionar dois fornecedores; o sistema deve impedir a segunda seleção.
4. **Filtro de bloqueio** – Marcar um fornecedor com `A2_MSBLQL = '1'` (bloqueado) e garantir que ele **não** aparece na lista.
5. **Persistência** – Após confirmar a seleção, checar que o campo de destino foi corretamente preenchido e que a tela foi refrescada (`SysRefresh`).
