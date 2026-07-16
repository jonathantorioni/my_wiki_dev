---
id: scoma040prw
title: SCOMA040.PRW
---

# SCOMA040.PRW

**Monitor de Recebimento XML / NF-e - rotina específica Shark**

----

### Dados da customização

Analista responsável: Wagner Mobile Costa (rotina original) / Manutenções por Jonathan Torioni, Rafael Rondon e demais analistas

----

### Especificação da customização

Rotina específica do sistema de compras Shark responsável por acompanhar e tratar o recebimento de documentos fiscais via XML (NF-e, CT-e, NFS-e, veículos e chassi). Atua sobre a tabela `SZG` (Monitor de Documentos Eletrônicos) oferecendo recursos de visualização, libera­ção/rejeição manual, consulta SEFAZ pela chave, desmembramento de notas, controle fiscal multi-CFOP, manutenção de chassi, geração de pré-nota (`SF1`), revalidação de regras fiscais, exporta­ção do XML e auditoria de consultas via tabela `LEGBRANCA` (tratamento da rejeição **656 - Consumo Indevido**).

----

### Especificação de tabelas e índices

* **SZG** - Monitor de documentos eletrônicos (cabeçalho dos XMLs recebidos).
* **SZ1** - Cabeçalho/financeiro relacionado ao XML.
* **SZ2** - Itens do XML.
* **SZA** - Amarração de pedidos x notas.
* **SZP** - Cadastro de CNPJ x ID_ENT (TSS).
* **RZB** - Histórico/observações do monitor.
* **SF1 / SD1 / SE2** - Documentos de entrada gerados após a liberação.
* **SC7** - Pedidos de compra (validação de saldo).
* **SPED001 / LEGBRANCA** - Tabelas externas (SQL direto) utilizadas para identificar o ID_ENT do TSS e para auditoria das consultas SEFAZ.

----

### Especificação de parâmetros

* **ES_USRXMLI** - Lista de usuários com permissão para alterar dados do XML de importação.
* **ES_USRXML** - Lista de usuários com permissão para alterar dados de XML de CT-e.
* **ES_NEWCONS** - Define o uso da nova rotina de consulta SEFAZ (`U_SCNewConsSe`).
* **ES_QTDMEMO** - Tamanho máximo do memo de observações.
* **ES_XMLVIST** - Quando `.T.` filtra somente XML vistados pelo operador fiscal.
* **ES_XBXPCNF** - Controla a baixa do pedido de compra na entrada da NF.
* **ES_ESTQZER** - Define se subtrai `C7_QTDACLA` do saldo disponível do pedido.
* **ES_OPTRANS** - Operação default utilizada na transferência.
* **ES_OPEERCN** - Operação default de garantia.
* **ES_MQESTFI** - Indica uso do estoque financiado pela fábrica.
* **ES_EMPVFUND** - Empresas com tratamento de fundo de investimento.
* **ES_MQVFCI** - Habilita validação do FCI.
* **ES_LOCACAO** - Habilita tratamento de loca­ção.
* **ES_GRPVEI** - Grupos de produtos considerados veículo.
* **ES_DIASDUP** - Dias limite para duplicidade de venci­mento.
* **ES_OPSAENT** - Habilita altera­ção da operação de saída x entrada.
* **ES_OPEGARC** - Operação de garantia para validação de chassis.
* **ES_NFSELOK** - Habilita ok obrigatório para NFS-e.
* **ES_LIBMNA** - Libera consulta SEFAZ mesmo fora de produção.
* **ES_AUDLEG** - Habilita auditoria/gravação na tabela `LEGBRANCA` (rejeição 656 - Consumo Indevido).
* **ES_PRZCEL / ES_PRZCEL2 / ES_TRVNFS / ES_DTLIBCE** - Controles de trava de SPED/CTE/NFS-e.
* **MV_SPEDURL** - URL do TSS para consumo dos webservices SEFAZ.
* **MV_RESTNFE** - Bloqueio NFE x pedido de compra.

Grupo de perguntas: **SC040BRW** (filtros do browse, ajustado em runtime via `AjustaPerg`/`PutSx1`).

----

### Especificação das funções

* **U_SCOMA040** - Rotina principal. Monta o `aRotina`, ajusta perguntas, abre a tabela `SZG` e inicia o browse do monitor.

* **MenuDef** - Estática que retorna o `aRotina` do browse com as operações disponíveis (Visualizar, Liberar, Rejeitar, Consultar SEFAZ, Histórico Fiscal, etc.).

* **U_SC030Mnt** - Monta a tela de visualização dos dados do XML, com folders de cabeçalho, itens, financeiro e observações.

* **U_SC030Sef** - Realiza a consulta SEFAZ via `U_SCNewConsSe` e a atualização via `U_SCUpdZgSefaz`.

* **U_SC030Vld** - Revalida a situação do documento em relação a compras/fiscal.

* **U_SC030RNF** - Confirma o recebimento (gera SF1) após validações fiscais e bloqueio do fornecedor (`A1_MSBLQL` / `A2_MSBLQL`).

* **U_SC030Lib** - Trata o aceite ou rejei­ção da diverg­ência da NF (1 = libera, 2 = rejeita) com justificativa.

* **U_SC030Ob2 / U_SC030Obs** - Telas e gravação das anotações (`RZB`) realizadas pelos usu­ários durante a análise.

* **U_SC030Cmp / U_SC030Fis / U_SC030Itm** - Manuten­ções específicas de campos, fiscal e itens.

* **U_SC030Leg / U_SC030Flt / SC040LEGX** - Defini­ção e filtragem das legendas (vistado, divergência, etc.).

* **U_XmlDfPCOP** - Define o CFOP/operação default da nota a partir do grupo tribut­ário/grupo de produto.

* **U_XmlOpObg** - Obriga uma opera­ção espec­ífica de acordo com a regra.

* **U_XmlGetOp** - Retorna a opera­ção fiscal cabível para o item baseado em `Z2_CFOP`, `B1_TIPO`, `cGrTrib` e regras específicas (4108, 9710, 10178, garantia).

* **U_CodProd** - Localiza o código do produto a partir do CNPJ do fornecedor, loja e código no fornecedor.

* **WNZ2_COD / WNZ2_OPER / WNZ2_TES / WNZ2_NCM / WNZ2_CHASSI / WNZ2_NUMPC** - Gatilhos de campos do XML.

* **U_CodForn** - Localiza o código do fornecedor a partir do CNPJ/CPF/inscrição.

* **U_FCViewImp / U_FRetStSF** - Vis­ualiza­ção e retorno do status do SEFAZ.

* **U_SC30Mail** - Envia e-mail com as informa­ções do XML para análise.

* **U_SC040LDP / U_SC040TDP** - Lançamento/tratamento do pedido durante a libera­ção.

* **U_SC040XML** - Exporta o XML para diret­ório informado.

* **U_SC040LOK / U_SC040TOK** - Validações de OK/Tudo OK do browse.

* **U_SC040CMP / U_XMLCHASSI / U_SC40MUCH** - Manuten­ção de chassi/m­últiplos chassis.

* **U_SC40CLOK / U_SC40CTOK** - Va­lida­ções específicas para chassis.

* **U_SC040Tipo** - Alterna o tipo de nota controlado pela Static `cNF040Tipo` (P=Produtos, D=Documentos, V=Veículos).

* **U_SCNewConsSe** - Consulta a chave da NF-e no SEFAZ via `TWsdlManager` apontando para `MV_SPEDURL`. Quando `ES_AUDLEG` est­á habilitado integra-se com a tabela `LEGBRANCA` chamando `U_CON656`, `U_STMPLEG`, `U_INS656` e `U_STMPUPD` para evitar a rejei­ção 656.

* **U_SCGetIdEnt** - Retorna o `ID_ENT` do TSS para o CNPJ informado, consultando a tabela `SPED001` (com schema diferente por empresa) com `JOIN` em `SZP`.

* **U_SCUpdZgSefaz** - Atualiza na `SZG` o resultado da consulta SEFAZ retornada por `U_SCNewConsSe`.

* **U_SC040Chk / U_SC2DUPREF** - Validações de cabeçalho da NF e dupli­cidade.

* **U_LibCeL / U_LibdiaCel** - Liberação de trava do celular (controle SPED/CTE).

* **U_ChkSaldoPed** - Verifica o saldo do pedido de compra.

----

:::info

A rotina utiliza diversas tabelas SQL diretas (`SIGA_SPEDSK.SPED001`, `SIGA_SPEDPRD.SPED001`, `LEGBRANCA`) por meio de `TcSqlExec` e `MpSysOpenQuery`. As tabelas estão fora do dicionário Protheus.

:::

:::info

A integra­ção com o controle de **rejei­ção 656** (consumo indevido) é feita exclusivamente dentro de `U_SCNewConsSe`. Quando `ES_AUDLEG = .F.` o comportamento da consulta SEFAZ é o original.

:::
