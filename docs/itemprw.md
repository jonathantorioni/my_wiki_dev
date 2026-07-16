---
id: itemprw
title: ITEM.PRW
---

# ITEM.PRW

 **CUSTOMIZAÇÃO DE CADASTRO DE PRODUTOS [SB1/SBZ]**

### **Dados da Customização**

**Analista:** Fernando Carvalho

----

### **Especificação da Customização**

Esta rotina foi desenvolvida como um Ponto de Entrada (User Function) para o modelo de dados de Produtos (SB1), utilizando o framework MVC. O objetivo principal é a automação e sincronização de dados tributários e descritivos nas tabelas **SB5** (Dados do Produto) e **SBZ** (Indicadores de Produtos) durante os processos de inclusão e alteração.

A lógica é acionada especificamente no ponto de controle **FORMPOS** (Pós-Posicionamento do Formulário), garantindo que, após o preenchimento dos dados do produto, as informações fiscais sejam calculadas via classe `GRTRIBPR` e replicadas para as filiais correspondentes.

----

### **Especificação de Tabelas e Índices**

* **SB1**: Tabela mestre de Produtos, utilizada para leitura de dados básicos (NCM, Tipo, Grupo) e gravação de campos tributários.
* **SB5**: Dados Complementares do Produto, atualizada com a descrição (B5_CEME) do item.
* **SBZ**: Tabela de Indicadores de Produtos, utilizada para armazenar detalhes fiscais por filial (ICMS, PIS, COFINS, etc.).

----

### **Especificação de Parâmetros**

Não foi identificado o uso de parâmetros de sistema (`GETMV`) específicos neste fonte.

----

### **Especificação de Perguntes**

Não houve a utilização de grupos de perguntas (`Pergunte`) nesta rotina.

----

### **Especificação das Funções e Rotinas**

**[U_ITEM]** - Ponto de Entrada acionado pelo modelo MVC de Produtos.
* **Parâmetros**: Recebe o array `PARAMIXB`, contendo o objeto do modelo, o ID do ponto de controle e o ID do modelo.
* **Funcionamento**: 
    1. Valida se o ponto acionado é o **FORMPOS**.
    2. Identifica a operação ativa (3-Inclusão ou 4-Alteração).
    3. Instancia a classe `GRTRIBPR` para processamento fiscal.
    4. Realiza a validação da NCM (SYD) e interrompe o processo caso seja inválida.
    5. Percorre as filiais definidas para atualizar ou incluir registros nas tabelas **SB5** e **SBZ**.
    6. Atualiza os campos do modelo mestre (**SB1MASTER**) com os valores calculados (Aliquotas de ICMS, PIS, COFINS, etc.).

----

### **Roteiro de Homologação**

**Atualização de Dados Fiscais**
1. Acessar o Cadastro de Produtos (MATA010) via Protheus.
2. Realizar a inclusão de um novo produto do tipo "ME" ou "PV"
3. Informar os dados obrigatórios, incluindo NCM e Classe
4. Ao confirmar (clicar em Salvar), o sistema deve validar a NCM.
5. Verificar se foram criados/atualizados registros na tabela **SB5** com a descrição correta.
6. Verificar se os registros na tabela **SBZ** foram gerados para as filiais alvo com as alíquotas de impostos preenchidas.
7. Validar se os campos tributários no cabeçalho do produto (SB1) foram preenchidos automaticamente conforme o retorno da classe fiscal.

**Validação de NCM Inválida**
1. Tentar incluir ou alterar um produto informando uma NCM inexistente na tabela SYD.
2. O sistema deve exibir uma mensagem de erro ("NCM INVÁLIDA") e impedir a gravação do registro.
