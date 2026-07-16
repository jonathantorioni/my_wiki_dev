---
id: menvmailprw
title: MENVMAIL.PRW
---

# MENVMAIL.PRW

**Envio de e-mail assíncrono Protheus**

### **Dados da Customização**

----

Analista: Jonathan Torioni

----

### **Especificação da Customização**

----


Está customização tem o objetivo de realizar o envio de e-mails assíncronos.

Inicialmente está customização foi desenvolvida para destravar as liberações, visto os problemas recorrentes de lentidão na plataforma G-Suite (Google - Gmail).

Todos os e-mail serão enviados via AWS, visando ter mais simplicidade nos envios e nas autenticações das contas. Esta decisão foi tomada, pois após algumas autenticações com a Google - Gmail, a plataforma começou a bloquear os "Aplicativos menos seguros".


----

### **Especificações das Funções e Rotinas**

----

**U_MENVMAIL(_aParam)** - Utilizada para realizar a integração com o envio de e-mail assíncrono. Esta função é responsável por pegar as informações de (Origem, Destino, Assunto, Corpo do e-mail e Anexos).

Esta função também é responsável por criptografar os dados da Conta, Senha e Servidor SMTP do AWS e gravar os dados na tabela PCM.

**U_MPRCMAIL()** - Função responsável pela separação do recnos da tabela PCM que contenham a coluna PCM_STATUS em 1 ou 4. 

Está função será configurada no JOB com um refresh rate de um minuto.

**U_MSEND(_cEmp,_cFil, _aRecno)** - Função responsável pelo envio de e-mail dos recnos passados pela função U_MPRCMAIL()

**Observações**

As funções U_MPRCMAIL e U_MSEND são feitas exclusivamente para execução via JOB.

----

### **Especificações de tabelas/campos/parâmetros**

----
Alias|Campo|Tipo|Tamanho|Decimal|Mascara|Nome|Descrição
---|----------|-|---|-|---------------------------------------------|------------|--------------------
PCM|PCM_EMPORI|C|2  | |@!                                           |EMP ORIG    |EMPRESA ORIGEM
PCM|PCM_SMTPPW|C|254| |@!                                           |SENHA SMTP  |SENHA SMTP 
PCM|PCM_SMTPSV|C|254| |@!                                           |SERVER SMTP |SERVER SMTP
PCM|PCM_SMTPCT|C|254| |@!                                           |CONTA SMTP  |CONTA SMTP
PCM|PCM_STATUS|C|1  | |@!                                           |STATUS ENV  |STATUS ENVIO
PCM|PCM_FROM  |C|100| |@!                                           |FROM        |FROM
PCM|PCM_DEST  |M|10 | |@M                                           |DESTINATARIO|DESTINATARIO
PCM|PCM_ASSUN |C|254| |@!                                           |ASSUNTO     |ASSUNTO
PCM|PCM_BODY  |M|10 | |@M                                           |BODY        |BODY
PCM|PCM_ATTAC |C|254| |@!                                           |ANEXO       |ANEXO
PCM|PCM_CUSRID|C|6  | |@!                                           |ID USUARIO  |ID USUARIO
PCM|PCM_RC4   |C|99 | |@!                                           |CHAVE RC4   |CHAVE DECRIP RC4
PCM|PCM_DTIN  |D|8  | |@D                                           |DATA INC    |DATA INCLUSAO DO ENVIO
PCM|PCM_HRIN  |C|10 | |@!                                           |HORA INC    |HORA INCLUSAO
PCM|PCM_DTOUT |D|8  | |@D                                           |DATA SAIDA  |DATA DE SAIDA
PCM|PCM_HROUT |C|10 | |@!                                           |HORA SAIDA  |HORA SAIDA
PCM|PCM_MSERR |C|99 | |@!                                           |MSG ERRO    |MENSAGEM DE ERRO
PCM|PCM_ROTINA|C|50 | |@!                                           |ROTINA      |ROTINA
A tabela PCM deve ser criada com compartilhamento entre filiais, empresas e grupos com o nome PCMG01.
Após a criação da tabela, deve-se rodar a criação de campos utilizando a ferramenta MDIC importando o arquivos schema_campos_pcm.txt na pasta \system.

A tabela PCM deve ser importada na SX2 de todas as empresas, juntamente com a tabela SIX.

Criar o parâmetro ES_MAILASS na tabela SX6 de todas as empresas da producao27 e producaosk. Este parâmetro será responsável por definir se o envio de e-mail será de forma assíncrona ou da forma tradicional.
Ao criar este parâmetro, deixe o conteúdo como .F. para que após a implantação, possamos controlar o envio por empresas e testar a estabilidade da aplicação.

----

### **Critérios para Validação**

----

Efetivar a liberação e verificar se as informações do e-mail em questão se encontram na tabela PCM com o Status 1.

Após isso rodar a função U_MPRCMAIL() e verificar se o status altera de 1 para 6.

**Status:**

    1 - EMAIL RECEBIDO
    4 - EMAIL NÃO TRANSMITIDO
    6 - EMAIL ENVIADO

Para os e-mails com status 4, é informada o motivo da falha na transmisão na coluna PCM_MSERR.
Caso o envio de e-mail esteja com o status 4, verifique se dentro do appserver.ini consta a Session [MAIL].

    [MAIL]
    AUTHLOGIN = 1
    AUTHNTLM  = 1
    AUTHPLAIN = 1
**Documentação oficial**: https://centraldeatendimento.totvs.com/hc/pt-br/articles/360016727371-MP-ADVPL-ERRO-THE-USER-COMMAND-FAILED
