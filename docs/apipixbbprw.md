---
id: apipixbbprw
title: APIPIXBB.PRW
---

# APIPIXBB.PRW

**APIPIXBB.PRW - API PIX - Banco do Brasil**

----

### Dados da customização

Analista responsável: Fernando Gabriel

----

### Especificação da customização

Este fonte Reúne endpoints destinados a lidar com gerenciamento de cobranças imediatas. 

----

### Especificação de tabelas e índices

Não possui

-----

### Especificação de parâmetros

**ES_URLPBB** = Url do método PUT para cobrança imediata pix

**ES_APPKBB** = Credencial de acionamento das APIs BB

**ES_URLGBB** = Url do método GET para consulta de pagamento

**ES_URLTBB** = Url do método POST access token

**ES_CLIEID** = Chave ID para utilização das API BB

**ES_SECRID** = Chave de segurança privada das API BB

----

### Especificação das funções

PutCobPix(oPag): Método PUT cobrança Pix - Banco do Brasil

Responsável por criar cobrança imediata.

#### Dados para Envio:

```json
{  
"calendario": {  
	"expiracao": tempo de validade do QR CODE, tamanho 5 "36000" },  
	"devedor": {  
	"cpf": CPF do pagador, tamanho 11 "12345678909",  
	"nome": nome do pagador, tamanho 40 "Francisco da Silva"},  
	"valor": {  
	"original": valor do recebimento, tamanho 6 "11.54"},  
	"chave": chave PIX para recebimento, tamanho 63 "7f6844d0-de89-47e5-9ef7-e0a35a681615",  
	"solicitacaoPagador": nome do pagador ou alguma observação, tamanho 40 "Francisco da Silva"
}
```

----

#### Dados de retorno:

**STATUS:  200 OK**

Cobrança imediata criada

```json

{
  "calendario": {
    "criacao": "2020-09-09T20:15:00.358Z",
    "expiracao": 3600
  },
  "txid": "7978c0c97ea847e78e8849634473c1f1",
  "textoImagemQRcode": dados para gerar a imagem do QR CODE do cobrança "00020101021226870014br.gov.bcb.pix2565qrcodepix-h.bb.com.br/pix/v2/92c63919-6902-4e3e-a828-49b80a713d005204000053039865802BR5920ALAN GUIACHERO BUENO6008BRASILIA62070503***6304EB96",
  "revisao": 0,
  "loc": {
    "id": 789,
    "location": "pix.example.com/qr/v2/9d36b84fc70b478fb95c12729b90ca25",
    "tipoCob": "cob"
  },
  "location": "pix.example.com/qr/v2/9d36b84fc70b478fb95c12729b90ca25",
  "status": "ATIVA",
  "devedor": {
    "cnpj": "12345678000195",
    "nome": "Empresa de Serviços SA"
  },
  "valor": {
    "original": "567.89"
  },
  "chave": "a1f4102e-a446-4a57-bcce-6fa48899c1d1",
  "solicitacaoPagador": "Informar cartão fidelidade"
}
```

**GetCobPix(cTxId)**: Método GET cobrança Pix - Banco do Brasil

Responsável pela consulta de cobrança imediata através de um determinado txid.

----

#### Dados para Envio:

cTxId: identificador do recebimento PIX, tamanho 35 “wyzr3mIYIOimncu2e0FPW6ZGfIE4ul3XGKi”

----

#### Dados de retorno:

**STATUS:  200 OK**

Dados da cobrança imediata.

```json
{
  "calendario": {
    "criacao": "2020-09-09T20:15:00.358Z",
    "expiracao": 3600
  },
  "txid": "7978c0c97ea847e78e8849634473c1f1",
  "revisao": 0,
  "loc": {
    "id": 789,
    "location": "pix.example.com/qr/v2/9d36b84fc70b478fb95c12729b90ca25",
    "tipoCob": "cob"
  },
  "location": "pix.example.com/qr/v2/9d36b84fc70b478fb95c12729b90ca25",
  "status": se o pagamento foi confirmado "CONCLUIDA",
  "devedor": {
    "cnpj": "12345678000195",
    "nome": "Empresa de Serviços SA"
  },
  "valor": {
    "original": "567.89"
  },
  "chave": "a1f4102e-a446-4a57-bcce-6fa48899c1d1",
  "solicitacaoPagador": "Informar cartão fidelidade"
}
```
