---
id: relabastprw
title: RELABAST.PRW
---

# RELABAST.PRW

**Relatorio de Abastecimento e Desabastecimento automático**

### Dados da customização

Analista responsável: Rafael Gomes

----

### Especificação da customização

Este fonte tem como objetivo enviar relatorio de pedido e itens do abastecimento e desabastecimento automático de mercadoria.

----

### Especificação de funções e rotinas

* **U_RELABAST** - Funcao responsavel para pegar os registros de pedidos e itens gerados para abastecimento e desabastecimento

* **MONTAREL** - Monta arquivo Excel com a extenção xml, (O arquivo é gerado em XML para que possa conter 2 ou mais abas no arquivo excel).

* **ABASMAIL** - Monta o Email que será enviado para usuario informados no grupo de usuario da PW7 (RELABAST)

----

### Especificação de parametros

Nenhum


:::info
Este fonte foi desenvolvido para rodar apenas via schedule.
:::
