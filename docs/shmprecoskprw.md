---
id: shmprecoskprw
title: SHMPRECOSK.PRW
---

# SHMPRECOSK.PRW

**Processo de nova formação de preço de venda SK / Ecommerce**

### Dados da customização

Analista responsável: Jonathan Torioni

----

### Especificação da customização

Para que a plataforma de ecommerce possa praticar os preços que atualmente são utilizados nas lojas físicas, surgiu a necessidade de ajustar a formação de preços no pedido de vendas.

----

### Critérios da customização

**Formação de Preço de Venda**
* Lista de Preço por UF - Continua existindo e tendo como base o PBV
* Fator por segmento de cliente ATACADO ou VAREJO - continua existindo
* Fator Frete - **Desaparece**
* Fator Financeiro (boleto) - **Desaparece**
* Fator Cliente Especial - **Desaparece**
* Faixas de Preços (1-2-3-4-5-6) - **Desaparece, onde no sistema todas essas faixas de preços terão fator 1,00**
* Fator Prazo de Pagamento - **Continua existindo, porém com um novo cálculo para os fatores em cada prazo**

Conforme solicitado foram criados dois parâmetros para realizar o controle do calculo desconsiderando, Fator Frete, Fator Financeiro, Fator Cliente Especial e Faixas de Preços (1-2-3-4-5-6).

----

### Especificação de funções

**U_SHMPRECOSK** - Função responsável pela montagem dos preços praticados na SK para exibição no Ecommerce

**RETUFS** - Funçao responsável por retornar todas as uf que a SK atual

**U_FORMPRCSK** - Função responsável por realizar a formação de preço da UF informada

----

### Customização de tabelas/capos

**Tabela PD4 - FILIAL X CROSSDOCKING**

Tabela|Ordem|Campo     |Tipo |Tamanho|Decimal
------|-----|----------|-----|-------|------|
PD4   |	01  |PD4_FILIAL|	C|2      |0
PD4   | 02  |PD4_ID    |	C|8      |0
PD4   |	03  |PD4_FILDES|	C|2      |0
PD4   |	04  |PD4_CODFIL|	C|2      |0
PD4   |	05  |PD4_TRANSP|	C|6      |0

----

### Especificação do funcionamento

Para o correto funcionamento a função U_SHMPRECOSK foi desenvolvida para ter o funcionamento em schedule.

Ao ser iniciada, o sistema distribui uma thread de processamento para cada UF onde a empresa SK possui participação ativa no mercado. Essas Threads são gerenciadas através de arquivos de texto localizados dentro do path **"\interf\tmp\ecommerce\"**, ao termino da execução os dados sao gravados dentro da tabela PD4 e os arquivos são automaticamente apagados por cada thread.
