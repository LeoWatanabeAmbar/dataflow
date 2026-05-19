# SC6G10 — Dicionário de Campos

## Objetivo

Documentar os principais campos operacionais da entidade:

```text
SC6G10 — Itens dos Pedidos de Venda
```

O objetivo deste documento é centralizar:
- significado semântico dos campos
- comportamento operacional
- observações importantes
- riscos conhecidos
- direcionamentos analíticos futuros

---

# Campos Operacionais

| Campo | Nome Semântico | Descrição | Observação |
|---|---|---|---|
| C6_NUM | Número do Pedido | Identificador operacional do pedido de venda | Chave comercial do pedido |
| C6_ITEM | Item do Pedido | Sequencial do item dentro do pedido | Define granularidade do item |
| C6_PRODUTO | Código do Produto | Código do produto associado ao item | Relacionamento com SB1G10 |
| C6_DESCRI | Descrição do Produto | Descrição operacional do produto no momento do pedido | Pode divergir do cadastro atual do produto |
| C6_QTDVEN | Quantidade Vendida | Quantidade comercializada do item | Unidade depende do cadastro do produto |
| C6_PRCVEN | Preço Unitário de Venda | Valor unitário comercial do item | Pode sofrer alterações fiscais/comerciais posteriores |
| C6_VALOR | Valor do Item | Valor total operacional/comercial do item | Não representa necessariamente receita líquida |
| C6_CF | CFOP do Item | Código Fiscal de Operações e Prestações do item | Utilizado para classificação fiscal/comercial |
| C6_NOTA | Número da Nota Fiscal | Número da nota fiscal vinculada ao item do pedido | Indica faturamento operacional do item |
| C6_DATFAT | Data de Faturamento Operacional | Data operacional relacionada ao faturamento do item | Não garante faturamento efetivo |
| C6_ENTREG | Data de Entrega | Data prevista de entrega do item | Utilizado em análises logísticas e operacionais |
| C6_BLQ | Status de Bloqueio | Indicador operacional de bloqueio do item | Itens com C6_BLQ = 'R' não devem ser considerados válidos |
| D_E_L_E_T_ | Exclusão Lógica | Indicador de exclusão lógica do ERP TOTVS | Registros deletados logicamente devem ser desconsiderados |

---

# Campos Relacionais

| Campo | Relacionamento | Entidade | Campo Entidade |
|---|---|---|---|
| C6_NUM | pedido | SC5G10 | C5_NUM |
| C6_PRODUTO | produto | SB1G10 | B1_COD |
| C6_NOTA | faturamento operacional | SD2G10 | D2_DOC |

---

# Regras Operacionais Identificadas

## Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

Registros deletados logicamente não deverão compor a camada analítica oficial.

---

## Elegibilidade operacional

```sql
C6_BLQ <> 'R'
```

Itens bloqueados operacionalmente não deverão ser considerados válidos para análises oficiais.

---

# Regras Semânticas Identificadas

## Indicação de faturamento

A presença de:

```text
C6_NOTA
```

indica que o item passou por processo operacional de faturamento.

---

## Data operacional de faturamento

A presença de:

```text
C6_DATFAT
```

não garante faturamento efetivo.

A validação oficial do faturamento deverá considerar:
- entidades fiscais oficiais
- elegibilidade operacional
- regras fiscais futuras

---

# Classificação Fiscal

O CFOP do item será utilizado para classificação inicial da natureza da operação.

Classificações previstas:
- VENDA
- REMESSA
- DEVOLUÇÃO
- TRANSFERÊNCIA
- BONIFICAÇÃO

A classificação definitiva deverá futuramente ser centralizada em:
```text
marts.dim_cfop
```

ou

```text
seeds/cfop_classificacao.csv
```

---

# Riscos Identificados

- preenchimento operacional inconsistente
- dependência de campos manuais
- divergência entre pedido e faturamento
- múltiplos faturamentos por item
- divergência entre CFOP operacional e fiscal
- pedidos parcialmente faturados

---

# Observações Importantes

Um item de pedido poderá:
- não ser faturado
- ser faturado parcialmente
- gerar múltiplos faturamentos
- possuir faturamentos em datas diferentes

O CFOP presente no pedido possui natureza operacional e poderá divergir do CFOP efetivamente utilizado no documento fiscal.

Os campos operacionais do ERP não deverão ser utilizados isoladamente como definição oficial de métricas analíticas sem validação das regras de negócio corporativas.

---

# Direcionamento Futuro

Os campos documentados nesta entidade deverão servir futuramente como origem para:
- modelos STAGING
- modelos MARTS
- regras centralizadas no dbt
- métricas oficiais
- camada semântica corporativa

Estrutura prevista:

```text
RAW → STAGING → MARTS
```