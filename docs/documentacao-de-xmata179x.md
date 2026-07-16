---
id: documentacao-de-xmata179x
title: Documentação de XMATA179X
---

# Documentação de XMATA179X

**Analista:** Jonathan Everton Torioni Oliveira

---

## Visão geral

O arquivo **xmata179x.prw** contém a implementação de um módulo Protheus (ADVPL) responsável pela gestão da matriz de abastecimento, incluindo a tela de navegação, definição de menus, modelo de dados e visualizações.

---

## Funções principais

| Função | Tipo | Descrição |
|--------|------|-----------|
| XMATA179X | User Function | Cadastro dos campos de controle da matriz de abastecimento; inicializa o browse, define filtros e ativa a tela. |
| MenuDef | Static Function | Define o menu funcional com as opções de ação da tela (Pesquisar, Gerar Sugestão, Visualizar, Alterar, etc.). |
| XT179EfeX | User Function | Manipula o fluxo de finalização da análise, alterando o status da sugestão conforme a escolha do usuário. |
| XMT179Vis | User Function | Exibe a visualização da tela de processamento usando `FWExecView`. |
| XA179ChanM | User Function | Alterna entre visualização e edição de acordo com o status da sugestão. |
| ModelDef | Static Function | Cria e configura o modelo de dados, definindo estruturas, campos, grids e validações. |
| ViewDef | Static Function | Define a view associada ao modelo, configurando grupos, boxes, abas e mapeamento de campos. |
| A179VLMod | Static Function | Validação de operação do modelo, impedindo alterações ou exclusões quando a sugestão já está efetivada. |
| A179FilAct | Static Function | Carrega os grids de filtros (tipo, grupo, marca) e preenche dados de apoio ao abrir a tela. |

---

## Detalhes das funções

### XMATA179X (User Function)

```advpl
User Function XMATA179X()
```
Responsável por montar o browse (`FWMBrowse`) da tabela **DBJ**, aplicar filtros de acordo com a opção selecionada pelo usuário e ativar a tela.

### MenuDef (Static Function)

```advpl
Static Function MenuDef()
```
Constrói o vetor de opções do menu, vinculando cada ação a um comando específico do Protheus.

### XT179EfeX (User Function)

```advpl
User Function XT179EfeX()
```
Gerencia a finalização da análise de sugestão, atualizando o campo `DBJ_FLAG` e exibindo mensagens de confirmação.

### XMT179Vis (User Function)

```advpl
User Function XMT179Vis()
```
Executa a visualização da tela de processamento via `FWExecView`.

### XA179ChanM (User Function)

```advpl
User Function XA179ChanM()
```
Alterna entre as telas de visualização e edição dependendo do estado da sugestão.

### ModelDef (Static Function)

```advpl
Static Function ModelDef()
```
Define a estrutura do modelo de dados, criando campos, grids e configurando propriedades como chave primária e descrições.

### ViewDef (Static Function)

```advpl
Static Function ViewDef()
```
Configura a view associada ao modelo, organizando grupos, boxes, abas e campos na interface do usuário.

### A179VLMod (Static Function)

```advpl
Static Function A179VLMod(oModel)
```
Valida operações de atualização ou exclusão, impedindo modificações quando a sugestão está efetivada.

### A179FilAct (Static Function)

```advpl
Static Function A179FilAct(oModel)
```
Carrega os dados dos filtros (tipo, grupo, marca) ao iniciar a tela, incluindo consultas SQL para populações de grids.
