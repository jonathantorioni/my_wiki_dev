---
id: agimpprdcrmprw
title: agimpprdcrm.prw
---

# agimpprdcrm.prw

# Documentação das Funções para Integração de Produtos

## `agcrmcsv` function

Esta função exibe uma mensagem de alerta solicitando que o usuário confirme se o arquivo a ser importado pertence a empresa em que ele está atualmente logado. Caso o arquivo não pertença a empresa, o usuário deve sair da empresa atual e logar na empresa correspondente antes de prosseguir com a importação.

### Funcionalidade

- **Verificação de Empresa:** Garante que o arquivo a ser importado pertence a empresa correta antes de iniciar o processo de importação.
- **Alerta ao Usuário:** Exibe um alerta solicitando a confirmação do usuário, garantindo que ele esteja logado na empresa correta.

## `agcrmimp` function

Responsável por ler um arquivo `.CSV` e validar a presença das colunas obrigatórias: `GRUPO`, `MARCA`, `MODELO`, `PRODUTO`, e `DESCRICAO`. Após a validação, o conteúdo do arquivo é gravado nas tabelas `ZR1`, `ZR2`, `ZR3` e `ZR4`. Em seguida, a função inicia o processo de integração com o sistema Campos Dealer.

### Funcionalidade

- **Leitura de Arquivo:** Carrega o arquivo `.CSV` e valida a presença das colunas obrigatórias.
- **Gravação de Dados:** Os dados do arquivo são registrados nas tabelas internas `ZR1`, `ZR2`, `ZR3` e `ZR4`.
- **Início da Integração:** Após a gravação, inicia o processo de integração com o Campos Dealer.

## `IntGrpCD` function

Realiza a integração dos dados do `GRUPO` com o sistema Campos Dealer. Esta função envia as informações do `GRUPO` para o Campos Dealer, assegurando que os dados estejam corretamente sincronizados e atualizados no sistema parceiro.

### Funcionalidade

- **Integração de Dados:** Envia as informações do `GRUPO` para o Campos Dealer.
- **Sincronização:** Garante que os dados do `GRUPO` estejam sincronizados e atualizados no sistema Campos Dealer.

## `IntMarCD` function

Realiza a Integração dos dados da `MARCA` com o sistema Campos Dealer. Esta função envia as informações relacionadas a `MARCA` para o Campos Dealer, garantindo que os dados estejam corretamente sincronizados e atualizados no sistema parceiro.

### Funcionalidade

- **Integração de Dados:** Envia as informações da `MARCA` para o Campos Dealer.
- **Sincronização:** Garante que os dados da `MARCA` estejam sincronizados e atualizados no sistema Campos Dealer.

## `IntModCD` function

Realiza a Integração dos dados do `MODELO` com o sistema Campos Dealer. Esta função envia as informações relacionadas ao `MODELO` para o Campos Dealer, garantindo que os dados estejam corretamente sincronizados e atualizados no sistema parceiro.

### Funcionalidade

- **Integração de Dados:** Envia as informações do `MODELO` para o Campos Dealer.
- **Sincronização:** Garante que os dados do `MODELO` estejam sincronizados e atualizados no sistema Campos Dealer.

## `IntPrdCD` function

Realiza a Integração dos dados dos `PRODUTOS` com o sistema Campos Dealer. Esta função envia as informações dos `PRODUTOS` para o Campos Dealer, garantindo que os dados estejam corretamente sincronizados e atualizados no sistema parceiro.

### Funcionalidade

- **Integração de Dados:** Envia as informações dos `PRODUTOS` para o Campos Dealer.
- **Sincronização:** Garante que os dados dos `PRODUTOS` estejam sincronizados e atualizados no sistema Campos Dealer.

## `GetCDeal` function

Executa uma requisição HTTP `GET` ao sistema Campos Dealer. Esta função faz um `/GET` para recuperar informações do Campos Dealer, permitindo a obtenção de dados ou o status necessário para o processamento adicional.

### Funcionalidade

- **requisição de Dados:** Realiza uma requisição `GET` para obter informações do Campos Dealer.
- **Processamento de Resposta:** Processa os dados ou status retornados para uso em etapas subsequentes.

## `PostCDea` function

Executa uma requisição HTTP `POST` ao sistema Campos Dealer. Esta função faz um `/POST` para enviar dados para o Campos Dealer, permitindo a atualização ou criação de informações no sistema parceiro.

### Funcionalidade

- **Envio de Dados:** Executa uma requisição `POST` para enviar informações ao Campos Dealer.
- **atualização ou criação:** Permite a atualização ou criação de registros no sistema Campos Dealer.

## `CREDENCIAIS` function

Retorna as credenciais necessárias para a conexão com o sistema Campos Dealer. Esta função fornece as informações de autenticação e acesso requeridas para estabelecer comunicação e integrar com o Campos Dealer.

### Funcionalidade

- **Fornecimento de Credenciais:** Retorna as credenciais necessárias para autenticação no Campos Dealer.
- **Estabelecimento de conexão:** Utiliza as credenciais para estabelecer uma conexão segura com o sistema Campos Dealer.

## `TratChar` function

Remove caracteres indesejados de uma string. Esta função elimina caracteres específicos ou não desejados de uma entrada de texto, garantindo que a string resultante esteja formatada corretamente para processamento ou armazenamento.

### Funcionalidade

- **Sanitização de String:** Remove ou substitui caracteres indesejados em uma string.
- **Formatação Correta:** Garante que a string esteja formatada adequadamente para processamento ou armazenamento subsequente.
