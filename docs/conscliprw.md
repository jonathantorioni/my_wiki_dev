---
id: conscliprw
title: CONSCLI.PRW
---

# CONSCLI.PRW

 **CONSPRO.PRW - CONSCLI[WEBAPP]**

### **Dados da Customização**

**Analista:** Jonathan Everton Torioni Oliveira

----

### **Especificação da Customização**


Em virtude da migração do sistema Protheus para infraestrutura CLOUD, o sistema de consulta lista de preço deve ser ajustado para o seu funcionamento no **Protheus WebApp**. Devendo manter o mesmo funcionamento com o minimo de ajuste e mudança nos processos possível.

----

### **Especificação de Tabelas e Índices**

Não foi realizado alterações em tabelas/indices.

----

### **Especificação de Parâmetros**

Não foi realizado ajustes em parâmetros.

----

### **Especificação de Perguntes**

Não houve modificações em perguntes.

----

### **Especificação das Funções e Rotinas**

**[U_CONSLOGIN]** - Função desenvolviva para solicitar o e-mail e o CNPJ dos nossos fornecedores antes de acessar o sistema de consulta. 

A utilização do Protheus WebApp implicou em uma mudança significativa devido as suas limitações. O Protheus WebApp não possui acesso ao sistema de gerenciamento de arquivos do Windows ou qualquer outro OS. Para maiores informações: [Diferenças entre as versões HTML e Desktop](https://tdn.totvs.com/pages/viewpage.action?pageId=118885352)

Devido a característica principal do **Conscli** em não realizar a solicitação de login e senha para acesso a lista de preço, o sistema foi desenvolvido com base em gerenciamento de acessos com o arquivo **conspro.txt** localizado em: **C:\\listaPreco\\** (pasta de instalação do Conscli) e na liberação dos acessos através da tabela PB1G01.

As limições do Protheus WebApp causaram um impacto significativo neste sistema de genrenciamento de arquivos, visto que, a aplicação passa a não possuir mais o acesso ao sistema de gerenciamento de arquivos do OS. Para que todas as funcionalidades do customização continuasse a entregar o resultado esperado e também para que os nossos parceiros não necessitassem realizar um novo pedido de liberação as areas responsáveis, foi necessário transferir toda gestão de arquivos para o servidor da aplicação.

A partir do momento em que o conscli passar a funcionar em WEBAPP, o gerenciamento ficará na pasta **\\interf\\conscli\\**.

Com o conscli funcionando no Protheus WebApp, os parceiros que realizam a consulta na lista de preço, deverão informar apenas o **E-mail cadastrado na liberação anterior** e o **CNPJ** da empresa. Com essas informações, o sistema irá criar automaticamente a pasta e as preferências de idioma do parceiro na pasta indicada anteriormente.

**Formato do gerenciamento:**

Dentro da pasta anteriormente informada, cada cliente passará a ter sua "propria pasta", ao acessar pela primeira vez o sistema via Protheus WebApp, o sistema irá criar a pasta com o seguinte formato: **E-mail + CNPJ** Ex: **teste_teste.com.br_77071329000178** o arroba será subustituido por underline.

Dentro da pasta do parceiro, conterá os arquivos: 
- **conspro.txt** - Arquivo de acessos as empresas (criptografado).
- **conspro-idioma.txt** - Arquivo de preferência de idioma (P-Portugues | E-Espanhol)

Caso o parceiro já possua liberações feitas anteriormente, o sistema trará automaticamente os acessos, caso contrário, aparecerá uma mensagem na tela do usuário perguntando se ele deseja solicitar a liberação da consulta lista de preço.

Após a solicitação, a equipe responsável, ficará encarregada de fazer a liberação do parceiro. Ressalto que nenhum processo interno foi alterado, somente a tela de acesso a lista de preços.

----

### **Especificação de Fontes**

**[CONSPRO]** - Lista para consulta de produtos.

----

### **Critérios para Validação**

**Homologação cadastro antigo**
1. Realizar o acesso com um E-mail e CNPJ que já estejam liberados. (Informação pode ser obtida na tabela PB1)
2. Verificar se as empresas que o parceiro tem acesso foram carregadas.
3. Consultar um ou mais produtos e verificar se a consulta trouxe a lista de preço somente para as empresas que o parceiro tem acesso.
4. Comparar com a execução do Conscli via SmartClient.

**Homologação cadastro novo**
1. Realizar a tentativa de acesso com um e-mail qualquer, porém com um CNPJ de um dos nossos fornecedores.
2. Solicitar a liberação deste CNPJ para uma de nossas empresas/filiais.
3. Entrar no sistema com o cadastro realizado, porém sem realizar a liberação.
4. Tentar consultar um código de produto. (O sistema não deve trazer nenhuma informação).
5. Realizar a liberação do cadastro solicitado.
6. Sair da rotina e entrar novamente com o cadastro realizado.
7. Verificar se a legenda foi alterada conforme a liberação realizada.
8. Consultar o código do produto (O sistema deve retornar alguma informação).
9. Comparar com a execução do Conscli via SmartClient.
10. Solicitar uma nova liberação para o cadastro atual.
11. Verificar se a informação nova solicatação foi gravada corretamente na tabela PB1.

**Obs:** Caso todos os passos estejam executando conforme o esperado, basta realizar a compilação do conspro.prw em ambiente produtivo, criar um serviço apartado com o environment **[CONSULTA]**.

Os ajuste foram feitos para o funcionamento no Protheus WebApp, porém existem ajuste que só serão possíveis de realizar quando tivermos um ambiente de homologação no Cloud TOTVS.


----
