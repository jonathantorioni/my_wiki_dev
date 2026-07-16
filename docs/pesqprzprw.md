---
id: pesqprzprw
title: PESQPRZ.PRW
---

# PESQPRZ.PRW

**Analista:** Rafael Gomes

## Visão geral

Este fonte fornece a funcionalidade de consulta de prazos baseada nas características de um produto específico. O processo identifica atributos como categoria, subgrupo, grupo, marca e curva ABC para determinar o prazo de atendimento (lead time) mais adequado através de uma hierarquia de regras de negócio.

----

## Funções principais

| Função | Tipo | Descrição |
|--------|------|-----------|
| `PESQPRZ(cProd, cFil)` | **User Function** | Coleta os atributos do produto na tabela `SB1` e sua curva ABC na tabela `PAQ`, chamando a lógica de retorno de prazo. |
| `RETPRZ(cPrd, cCateg, cSubGrb, cGrp, cMarca, cCurva, cFil)` | **Static Function** | Realiza a busca hierárquica na tabela `PCX` para encontrar o prazo configurado para o perfil do produto informado. |

----

### `PESQPRZ()`

```advpl
USER FUNCTION PESQPRZ(cProd, cFil)
    // ...
    // 1. Busca dados do produto (B1_COD, B1_XCATEG, B1_XSUBGR, B1_GRUPO, B1_XCLASSE) na SB1
    // 2. Consulta a curva ABC do produto na tabela PAQ para a filial informada
    // 3. Chama a função RETPRZ passando todos os atributos coletados
    // ...
RETURN nRes
```

- A função utiliza a tabela `SB1` para identificar as características do produto.
- Realiza uma consulta SQL na tabela `PAQ` para obter o último registro de curva ABC (`PAQ_ANOMES DESC`).
- Retorna o resultado processado pela função `RETPRZ`.

### `RETPRZ()`

```advpl
STATIC FUNCTION RETPRZ(cPrd, cCateg, cSubGrb, cGrp, cMarca, cCurva, cFil)
    // ...
    // 1. Consulta a tabela PCX para a filial informada
    // 2. Avalia cada registro de PCX contra os parâmetros do produto
    // 3. Armazena em um array (aDados) a correspondência encontrada (ex: Categoria, Marca, etc.)
    // 4. Ordena as correspondências e seleciona o primeiro valor válido (prioridade)
    // ...
RETURN nRes
```

- A função implementa uma lógica de priorização baseada em uma lista de correspondências (`aDados`).
- A ordem de prioridade é definida pela ordem de classificação do array após a análise de todos os registros da `PCX`.
- Caso o primeiro valor encontrado seja zero, o sistema utiliza o segundo valor disponível no registro de configuração (`PCX_PZMGER`).

----

## Requisitos de ambiente

- Tabelas `SB1`, `PAQ` e `PCX`.

----

## Dicas de homologação

1. Verificar se o produto informado possui cadastro correto na `SB1` e se os campos de categoria e grupo estão preenchidos.
2. Validar se a curva ABC está sendo recuperada corretamente da tabela `PAQ` para o mês mais recente.
3. Testar diferentes combinações de atributos (ex: apenas categoria, apenas marca, apenas curva) para garantir que a função `RETPRZ` está aplicando a hierarquia de prazos corretamente conforme a tabela `PCX`.
4. Confirmar se, quando um prazo específico (ex: por marca) é zero, o sistema está assumindo o prazo geral configurado.
