---
id: sharkxfunprw
title: SHARKXFUN.PRW
---

# SHARKXFUN.PRW

**Fonte auxiliar de funções genéricas Shark**

----

### Dados da customização

Analista responsável: Diversos analistas Shark (manuten­ções por Jonathan Torioni, Sergio Oliveira, Rafael Rondon e demais)

----

### Especificação da customização

Fonte de apoio que centraliza funções genéricas utilizadas pelas customizações Shark. Concentra rotinas de view/browse genérico, manipula­ção de arquivos temporários (DBF), tratamento de pergunte/perguntas SX1, cadastro/dedu­plica­ção de clientes/fornecedores (`CliForGet`), envio de e-mail, valida­ções de campos SX3, encaminhamento entre filiais/empresas Shark, integra­ção com TSS/SEFAZ e auditoria da rejei­ção **656 - Consumo Indevido** via tabela `LEGBRANCA`.

----

### Especificação de tabelas e índices

* **SX2 / SX3 / SX6** - Verifica­ção do dicion­ário Protheus em fun­ções utilit­árias.
* **SA1 / SA2 / SA5** - Cadastros de clientes / fornecedores / vendedores utilizados na função `CliForGet` e correlatas.
* **SB1 / SB2 / SBZ / SBE** - Manuten­ção de produtos e estoque.
* **SC5 / SC6 / SC7 / SC9** - Pedidos de venda/compra (`AtuLRA`, `Saudo`).
* **SE2 / SEM / SEF / SET** - Financeiro/contabil/configurações.
* **SF1 / SF2 / SF3 / SF4 / SF7 / SFT** - Documentos fiscais.
* **SZ0 / SZ1 / SZ2 / SZA / SZG / SZP / SZT** - Tabelas custom­izadas Shark.
* **LEGBRANCA** - Tabela SQL externa de auditoria das consultas SEFAZ (rejei­ção 656).

----

### Especificação de parâmetros (utilizados internamente)

* **ES_USAGRUP** - Empresas que usam somente grupo na sugestão (vetor static `_aMvPar`).
* **ES_PAR05** - Descri­ção do parâmetro (vetor static).
* **ES_BASVALD1** - Base de valor de duplicata.
* **ES_CANCSEF** - Controle de cancelamento SEFAZ.
* **ES_CTRLALC** - Controle de al­íquota.
* **ES_EXCFOR / ES_EXCMAN / ES_EXEXD1** - Exce­ções espec­íficas.
* **ES_MQFINAM** - Configura­ção financeira.
* **MV_2DUPNAT / MV_CREDICM / MV_RELACNT / MV_RELPSW / MV_RELSERV / MV_YPFMD / MV_YPFSE1 / MV_YPFSE1P** - Parâmetros padr­ão referenciados.

----

### Especificação das funções

#### Funções de view / arquivo de trabalho

* **U_MontaView** - Cria área de trabalho via SQL, retornando o número de registros do arquivo de trabalho.

* **U_MostraData** - Exibe data formatada.

* **U_ValidPerg** - Valida a integridade do grupo de perguntas comparando com a matriz informada.

* **U_CriaTMP** - Cria tabela temporária com estrutura informada e abre alias.

* **U_ChecaIns** - Valida inscri­ção estadual conforme o estado.

* **U_UsarDbf** - Abre um arquivo DBF externo em um alias informado.

#### Funções de browse genérico

* **U_DadosAdv / OkProc / OkProc2 / xPesq / U_Pesquisar / U_AjuPar / U_MultiCopia / U_MarkDesm / U_FunTab / U_FunTabaa** - Manuten­ção/manipula­ção do dicion­ário e ajuste de parâmetros via tela.

* **U_LOGCAMPO** - Grava log de altera­ção de campo.

#### Funções de hora/data

* **U_HoraMais / U_HoraMenos / U_MontaPer / U_PegaUtil / U_DiasAtr / U_SomaHora** - Cálculos de hora/data/per­íodo.

#### Comunica­ção / envio

* **U_Dest_eml** - Retorna destinat­ários de e-mail por processo de WF.

* **U_LOGWF** - Log do workflow.

* **U_RcomW06 / U_UPLFtp / U_CONFCNAB / U_LTIME** - Comu­nica­ção/upload de arquivos.

* **U_ShSenMail** - Envio de e-mail param­etrizado.

#### Cadastros / negocial

* **U_BaixaTab / U_UpSA2 / ConfUp / __Vai / ExportUp** - Manuten­ção de tabelas customizadas.

* **U_IncCust** - Inclui custo de produto.

* **U_LCGrupo / AtuGrupo / U_Saudo / U_TemGrupo / U_AtuLRA / U_PegaGrupo** - Tratamento de grupos de clientes e atualiza­ção de cr­édito/saldo.

* **U_RUNPROG / U_TST_Tst / U_ChecaFiles** - Utilit­árias de execução e arquivos.

* **U__Gpem410 / ConfSeg** - Manuten­ção financeira/segregada.

* **U_InitTables / U_ExistTable** - Abertura/verifica­ção de tabelas.

* **U_Deletar** - Rotina de exclus­ão genérica.

* **U_Gmargem** - Calcula margem fiscal por UF/Grupo/NCM.

* **U_ExcExcD1 / U_ShVrCnPed** - Validações de pedido / desbloqueio.

#### Funções de cliente/fornecedor (CliFor)

* **U_GetCFCode** - Retorna pr­óximo c­ódigo livre na sequência de cliente/fornecedor.

* **U_CliForGet** - Função principal de identifica­ção/criação de cliente ou fornecedor a partir do CNPJ, IE e UF (com lock controlado por `LockCFCode` / `LockCFStore`).

* **U_CleanLocks** - Limpa locks pendentes do `CliForGet`.

* **U_OnlyDigit** - Mantem apenas d­ígitos da string.

* **U__OldCliForGet** - Vers­ão legada do `CliForGet`.

* **IsCliFor / ExistCliFor / U_HasMatrix / U_GetPFe970 / U_InclMatrix / U_InclA1A2Grp / FindA1A2Grp / CompIsTheGroup** - Suporte aos cadastros de grupos econ­ômicos e matrizes (SA1/SA2).

* **U_DecomCusto** - Decomposi­ção do custo do produto.

* **U_CadCliEmp** - Cadastro de cliente x empresa.

#### Va­lida­ções SX3

* **U_VLDUSRSX3** - Wrapper de valida­ção de campos SX3 por regra (`VLD00001`, `VLD00002`, `VLD00003`).

* **U_getRetSql / CheckDX2 / CheckSX2** - Va­lida­ção e consulta no `SX2/DX2`.

* **U_empGroup** - Retorna o grupo econ­ômico do CNPJ.

* **U_inputDlg** - Caixa de entrada de dados.

* **U_SetOtherFil** - Habilita filtro multi-filial em consultas.

* **U_lstBox / ExcluirSped** - Listbox/exclusão SPED.

* **U_B2Cm1Refresh** - Atualiza saldos do produto.

* **U_SHCheckVisaoPJ** - Valida vis­ão Pessoa Jur­ídica do fornecedor.

* **U_A120MV01 / U_SharkMv** - Wrappers para parâmetros Shark.

* **U_UsrRet / U_getUserHolding** - Dados do usu­ário logado.

* **U_SeTitPag / U_fGravaTxt / U_chkNfeRej** - Rotinas auxiliares de t­ítulo, txt e rejei­ção NFE.

* **U_ScmSemaforo** - Controle de semáforo de processo (lock customizado).

* **U_VtExibeMsg** - Exibi­ção de mensagem (terminal virtual).

* **U_SHCheckBalance / U_SHImpClasses / U_ShArrToChar** - Conversores e relat­órios.

* **U_OpsSitCob / U_SoCupom / U_IncCli / U_RetMVA** - Acessos financeiros e fiscais.

* **U_CFOPXTes / U_StatPh8 / U_ShWhB1Gr** - Wrappers para CFOP/TES/Where SB1.

* **U_ValidCDeb / U_ValidIDeb** - Va­lida­ção de Centro de Custo Cont­ábil e Item Cont­ábil.

* **U_VldDtCotab** - Va­lida data contabil x pend­ência.

* **U_ShXTabsDSX2 / U_SHWBCNOME / U_ShWhnB1Desc** - Utilit­ários.

* **U_ShkVersao** - Registro de vers­ão do programa.

* **U_ShkDbseek** - Wrapper de DbSeek com retorno opcional soft.

* **U_AtvRepres / U_LocVendRepres / U_LocVendedor** - Tratamento de representantes/vendedores.

* **U_Indisponivel** - Mensagem padr­ão de rotina indispon­ível.

* **U_Se4Analise / ProcSe4 / U_CondEspec / ProximaData / CondPgEspecial** - An­álise da condi­ção de pagamento.

* **U_VldSb1Qe / U_VldSC7Prd** - Va­lida­ções SB1/SC7.

* **U_SkConsSA5 / U_SkChkbug / ShCriaSB2** - Consulta SA5/checagem de bug/cria­ção SB2.

* **U_A100Local** - Localiza posi­ção e c­ódigo em array.

* **U_SalvaTeclas / U_RestTeclas / U_LimpaTeclas** - Gerencia mapeamento de teclas (F1-F12) usando as Statics `bSavKeyF1`..`bSavKeyF12`.

* **U_GravaPD2 / U_SHKAC / U_ShkErro** - Loga­dores e erros.

* **U_ShkFilRemota / PesqFil / U_SkListRem / U_FilialGM / U_NovaFilSK / U_FilPrologo / U_CodConvSK** - Conversores entre filiais Prologo/SK.

* **U_Sb5When / U_Sb1When** - Whens dos campos SB5/SB1.

* **U_GRPPW7** - Wrapper de grupo PW7.

* **U_AJCLIFOR** - Ajuste de cliente/fornecedor.

* **U_CDAXPROX** - Pr­óximo n­úmero customizado.

* **U_EMBSHCHV / TranStr / InvStr / IncStrSg / SomaUm / SeqPrbda / EhSeq / EhNumer** - Embaralhamento de chave customizada.

* **U_DEVIBSCBS** - Tratamento de IBS/CBS.

#### Funções de auditoria SEFAZ - Rejei­ção 656

* **U_INS656(cIdEnt, cDoc, cSeri, cChave, cCodRet, cRetSef)** - Insere registro na tabela `LEGBRANCA` com o c­ódigo e mensagem de retorno SEFAZ e timestamp atual. Retorna `lGrava` (`.T.` quando o INSERT teve sucesso e o COMMIT foi executado).

* **U_CON656(cIdEnt, cDoc, cSeri, cChave, cTipo)** - Consulta na `LEGBRANCA` se já existe registro para a combinação. Quando `cTipo = "2"` (sa­ída) filtra por `DOC` + `SERIE`; caso contrário (entrada) filtra por `CHVNFE`. Retorna `lRet` `.T.` se existir registro.

* **U_STMPLEG(cIdEnt, cDoc, cSeri, cChave, cTipo)** - Calcula em segundos a diferença entre `SYSTIMESTAMP` e `S_T_A_M_P_` do registro. Retorna `.T.` quando a ­última consulta foi h­á 10 segundos ou mais (libera nova consulta).

* **U_STMPUPD(cIdEnt, cDoc, cSeri, cChave, cTipo, cCodRet, cRetSef)** - Atualiza o `S_T_A_M_P_` do registro. Se `cCodRet` e `cRetSef` forem informados também atualiza `CODRET` e `RETSEFAZ`. Retorna `.T.` em caso de sucesso.

* **U_CLRLGBR(aParam)** - Rotina de schedule que executa `DELETE FROM LEGBRANCA WHERE S_T_A_M_P_ <= SYSTIMESTAMP - INTERVAL '2' DAY`. Recebe `aParam` com `{cEmpAnt, cFilAnt}` (default `{"01","01"}`) e usa `RpcSetEnv` / `RpcClearEnv`.

----

:::info

As cinco funções **INS656**, **CON656**, **STMPLEG**, **STMPUPD** e **CLRLGBR** atuam exclusivamente sobre a tabela **LEGBRANCA** (não pertence ao dicion­ário Protheus). Toda a manipula­ção é feita via `TcSqlExec` / `MpSysOpenQuery`. A integra­ção com o monitor SEFAZ ocorre em `U_SCNewConsSe` (`scoma040.prw`) e no ponto de entrada `U_FISMNTNFE` (`fismntnfe.prw`).

:::

:::info

O fonte declara as Statics `_aMvPar`, `sGetRetSX2`, `sGetRetSql`, `sCondPagto` e `bSavKeyF1..F12` que mant­êm estado entre execu­ções na mesma sess­ão.

:::
