---
id: usersinatprw
title: USERSINAT.PRW
---

# USERSINAT.PRW

**Bloqueio de usuários inativos no Protheus**

----

## Dados da Customização

Analista: Carlos Henrique

----

## Especificação da Customização

Está customização foi desenvolvidada com o objetivo de bloquear usuários inativos por mais de 45 dias no sistema.

A rotina roda semanalmente e envia um e-mail com todos os usuários bloqueados.  

----

## Especificações das Funções e Rotinas

**U_USERSINAT(aParam)** - É utilizada para fazer a busca na tabela SZT através do campo ZT_ACESSO(último acesso no sistema) e compara com o último acesso na CV8 pelça função ULTACESS para garatir que não tenha logado no sistema.

1. É feito uma busca na tabela SZT considerando que a data de corte (45 dias) e com o status **L (Liberado)** 

2. Compara os acessos da busca com a CV8 utilizando a função **ULTACESS**, caso o último acesso seja menor que a data de corte, é gravado em uma variavel array.

3. É feito um RecLock na tabela SZT para alterar os campos **ZT_NOME,ZT_EMAIL,ZT_EMAIL2 e ZT_STATUS**. Em seguida é feito o bloqueio via API, pois não temos acesso direto aos usuários. 

:::info
**ULTACESS** - Função para validar o último acesso no sistema através da tabela (CV8 - Log de Processamento) fazendo a busca em todas as empresas do grupo.
:::
----

## Critérios para Validação

É necessário atender alguns critérios para que seja efetivado o bloqueio, sendo assim: 

* 45 dias dias sem acessar o sistema;
* Não bloquar usuário do coletor,caixa,faturamento ou administrador.

----

## Parâmetros utilizados

1. ES_ENVUSER - E-mail destinatário;
2. ES_NOBLOC  - Usuários que não podem ser bloqueados;
3. ES_DTINAT - Critério de dias determinado pela Diretoria para o bloqueio.
