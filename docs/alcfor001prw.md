---
id: alcfor001prw
title: ALCFOR001.PRW
---

# ALCFOR001.PRW

**Alçada Maquinas e Peças**

### Dados da customização

Analista responsável: Rafael Gomes

----

### Especificação da customização

Este ajuste tem como objetivo retirar codigos chumbados do fonte

----

### Especificação de parametros

**ES_USRMQPC** = Usuario autorizado a Alterar Alcada de MQ e PC    
**ES_GERPC** = Gerente do departamento       
**ES_DIREPC** = Diretor                    

### Execução do Processo

* Acesso a rotina
Acessar Modulo Maquinas 97
Atualizações => Compras => ADM Compras => Alçada

**-Diretor Peças** (coloque seu ID nos parametros **ES_USRMQPC** e **ES_DIREPC**)
Vai abrir uma tela conforme imagem abaixo, Clique no botão **[Altera Limite Gerente Peças]**, ao abrir uma tela com valor, altere o valor e apenas o Limite Gerente Peças deve ser alterado.

![fd](img/ALC01.png)

Clique no botão **[Altera Limite Diretor Peças]**, ao abrir uma tela com valor, altere o valor, Limite Compras no Mês e Saldo compra no Mês dever ser alterado, mantendo do Gerente de peças

![fd](img/ALC02.png)

**-Gerente Peças** (Tire seu ID dos parametros de diretor e coloque seu ID nos parametros **ES_GERPC** e **ES_ALCPECA**)
Vai abrir uma tela conforme imagem abaixo, Clique no botão **[Altera]**, ao abrir uma tela com valor, altere o valor e o limite maximo subordinados peças será alterado.

![fd](img/ALC03.png)

**-Diretor Maquinas** (coloque seu ID nos parametros **ES_DIREMAQ**)
Vai abrir uma tela conforme imagem abaixo, Clique no botão **[Confirma]**

![fd](img/ALC04.png)

Vai abrir uma tela conforme imagem abaixo, Clique no botão **[Maquinas]**

![fd](img/ALC05.png)

Vai abrir uma tela conforme imagem abaixo, Clique no botão **[Confirma]**

![fd](img/ALC06.png)

Vai abrir uma tela conforme imagem abaixo, Clique no botão **[Alterar limite]**, altere o valor e o valor do usuario posicionado ira ser alterado.

![fd](img/ALC07.png)
