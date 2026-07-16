---
id: cestbasprw
title: CESTBAS.PRW
---

# CESTBAS.PRW

**Gerar planilha de colaboradores assiduo que vai receber cesta basica como premio**

### Dados da customização

Analista responsável: Rafael Gomes

----

### Especificação da customização

Alteração no salario limite, a principio foi feito sem casas decimais, essa alteração foi inclusa as casas decimais

----

### Especificação de funções e rotinas

* **EXPCSV01** - Função para gerar o relatorio

* **GETEMP** - Função para retornar o codigo da empresa

* **GETCNPJ** - Função para pegar os 4 ultimos digitos da filial


----

### Especificação de parametros


:::info
Nenhuma
:::

----

### Execução do Processo

* Acesso a rotina
Relatorios => Beneficios => Cesta Basica

* Ao abrir a tela de Parametros, Altere o Ano Mês para 3 mês anterior, altere a data inicial para o dia 21 do mês informado até o dia 20 do mês seguinte
![fd](img/CESTBAS1.png)

* Vai abrir a tela para selecionar a pasta que deseja salvar o arquivo

![fd](img/CESTBAS2.png)

* Após selecionar a pasta vai apresentar a mensagem com o local que foi salvo, vá até a pasta e confere se a planilha gerou com dados.
