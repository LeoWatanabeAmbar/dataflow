# Entidade — fct_pedido_item

## Objetivo

Consolidar os itens dos pedidos de venda da operação comercial da empresa.

A entidade tem como objetivo centralizar:
- pedidos comerciais
- quantidades vendidas
- preços de venda
- carteira comercial
- informações operacionais
- status de faturamento

A fato será utilizada como principal origem para análises:
- carteira de pedidos
- volume vendido
- pedidos em aberto
- conversão de faturamento
- backlog comercial
- indicadores operacionais

---

# Granularidade

Cada registro representa:

```text
1 item do pedido de venda
```

Chave operacional:

```text
C6_NUM + C6_ITEM
```

---

# Origens

| Entidade | Objetivo |
|---|---|
| SC6G10 | Itens dos pedidos |
| SC5G10 | Cabeçalho dos pedidos |
| SA1G10 | Clientes |
| SB1G10 | Produtos |
| SA3G10 | Vendedores |
| SBMG10 | Grupo comercial/econômico |

---

# Relacionamentos Principais

| Entidade Relacionada | Tipo |
|---|---|
| dim_cliente | cliente do pedido |
| dim_produto | produto vendido |
| dim_vendedor | responsável comercial |
| dim_grupo_comercial | agrupamento econômico/comercial |
| fct_faturamento_item | faturamento do pedido |

---

# Principais Atributos

| Atributo | Origem | Descrição |
|---|---|---|
| numero_pedido | SC6.C6_NUM | Número do pedido |
| item_pedido | SC6.C6_ITEM | Sequencial do item |
| data_emissao | SC5.C5_EMISSAO | Data de emissão do pedido |
| codigo_produto | SC6.C6_PRODUTO | Produto vendido |
| codigo_cliente | SC5.C5_CLIENTE | Cliente do pedido |
| loja_cliente | SC5.C5_LOJACLI | Loja/unidade do cliente |
| vendedor_interno | SC5.C5_VEND1 | Vendedor interno |
| representante | SC5.C5_VEND2 | Representante comercial |
| executivo | SC5.C5_VENDE1 | Executivo comercial |
| uf_venda | SC5.C5_ESTE | Estado da venda |
| municipio_venda | SC5.C5_MUNE | Município da venda |
| cfop_pedido | SC6.C6_CF | CFOP operacional do pedido |
| numero_nota | SC6.C6_NOTA | Nota fiscal vinculada |
| data_faturamento_operacional | SC6.C6_DATFAT | Data operacional de faturamento |
| data_entrega | SC6.C6_ENTREG | Data prevista de entrega |

---

# Principais Métricas

| Métrica | Origem | Descrição |
|---|---|---|
| quantidade_vendida | SC6.C6_QTDVEN | Quantidade comercializada |
| preco_unitario | SC6.C6_PRCVEN | Preço unitário de venda |
| valor_item | SC6.C6_VALOR | Valor operacional do item |
| valor_frete | SC5.C5_FRETE | Valor total de frete do pedido |

---

# Regras Operacionais

## Exclusão lógica

```sql
SC5.D_E_L_E_T_ <> '*'
AND SC6.D_E_L_E_T_ <> '*'
```

Registros deletados logicamente não deverão compor a fato oficial.

---

## Elegibilidade operacional

```sql
C6_BLQ <> 'R'
```

Itens bloqueados operacionalmente não deverão ser considerados válidos para análises oficiais.

---

## Tipo de pedido elegível

```sql
C5_TIPO = 'N'
```

A camada analítica considera apenas pedidos comerciais válidos.

---

# Regras Semânticas

## Natureza da entidade

A entidade representa os itens comercializados nos pedidos de venda.

A fato possui natureza:
- comercial
- operacional
- logística

---

## Pedido operacional

O pedido representa uma intenção/operação comercial.

A existência do pedido não garante:
- faturamento
- entrega
- receita efetiva

---

## Relacionamento com faturamento

Um item do pedido poderá:
- não ser faturado
- ser faturado parcialmente
- gerar múltiplos faturamentos
- possuir faturamentos em datas diferentes

O relacionamento entre pedido e faturamento não é necessariamente 1:1.

---

## CFOP operacional

O CFOP presente no pedido possui natureza operacional/comercial.

Esse CFOP poderá divergir:
- do CFOP efetivamente faturado
- da classificação fiscal final

---

## Data de faturamento operacional

O campo:

```text
C6_DATFAT
```

não garante faturamento efetivo.

A validação oficial do faturamento deverá considerar:
- entidades fiscais
- documentos faturados
- regras fiscais oficiais

---

# Métricas Derivadas Futuras

A entidade poderá futuramente compor:
- carteira de pedidos
- backlog
- pedidos em aberto
- lead time comercial
- conversão de faturamento
- volume vendido
- previsões comerciais

---

# Riscos Identificados

- pedidos não faturados
- faturamentos parciais
- múltiplos faturamentos
- divergência entre pedido e faturamento
- alterações comerciais posteriores
- inconsistência operacional
- pedidos cancelados
- preenchimento manual inconsistente

---

# Observações Importantes

Os valores presentes na entidade possuem natureza operacional/comercial.

Os valores do pedido poderão divergir:
- do faturamento efetivo
- da receita líquida
- dos valores fiscais finais

A entidade representa uma visão operacional da venda antes da consolidação fiscal.

A análise oficial de faturamento deverá priorizar entidades fiscais oficiais.

---

# Direcionamento Futuro

A entidade deverá futuramente servir como base para:
- camada MARTS
- dashboards comerciais
- carteira de pedidos
- análises de backlog
- previsões comerciais
- indicadores operacionais
- modelos semânticos
- regras centralizadas no dbt

Estrutura prevista:

```text
RAW → STAGING → FCT_PEDIDO_ITEM
```