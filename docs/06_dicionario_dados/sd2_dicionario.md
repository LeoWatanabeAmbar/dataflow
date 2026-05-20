# SD2G10 — Dicionário de Campos

## Objetivo

Documentar os principais campos operacionais da entidade SD2G10, responsável pelos itens de faturamento/notas fiscais de saída no ERP TOTVS.

O objetivo deste documento é centralizar:
- significado dos campos
- comportamento operacional
- observações importantes
- riscos conhecidos
- direcionamentos analíticos futuros

---

# Visão Geral

A entidade SD2G10 representa os itens faturados/documentos fiscais de saída.

Cada registro representa:

```text
1 item faturado
```

A entidade é utilizada em:
- faturamento
- fiscal
- financeiro
- análises comerciais
- análises de margem
- análises tributárias

A SD2 é considerada uma das principais entidades para composição do faturamento operacional/fiscal.

---

# Campos Operacionais

| Campo | Nome Semântico | Descrição | Observação |
|---|---|---|---|
| D2_DOC | Número da Nota Fiscal | Identificador da nota fiscal | Documento fiscal |
| D2_PEDIDO | Número do Pedido | Pedido vinculado ao faturamento | Relacionamento com SC5/SC6 |
| D2_COD | Código do Produto | Produto faturado | Relacionamento com SB1G10 |
| D2_EMISSAO | Data de Emissão | Data de emissão fiscal | Formato "YYYYMMDD" |
| D2_CF | CFOP | Código fiscal da operação | Utilizado em classificações fiscais |
| D2_CLIENTE | Código do Cliente | Cliente vinculado ao faturamento | Relacionamento com SA1G10 |
| D2_LOJA | Loja do Cliente | Loja vinculada ao cliente | Complementa identificação |
| D2_CUSTO1 | Custo do Item | Custo operacional do item faturado | Utilizado em análises de margem |
| D2_VALICM | Valor de ICMS | Valor de ICMS do item | Tributo estadual |
| D2_VALIPI | Valor de IPI | Valor de IPI do item | Tributo federal |
| D2_DIFAL | Valor de DIFAL | Valor de diferencial de alíquota | Operações interestaduais |
| D2_VALIMP5 | Valor de PIS | Valor de PIS do item | Tributo federal |
| D2_VALIMP6 | Valor de COFINS | Valor de COFINS do item | Tributo federal |
| D2_VALFRE | Valor de Frete | Valor de frete do item faturado | Pode compor valor final |
| D2_ICMSRET | ICMS Retido | Valor de ICMS retido/substituição tributária | Regra fiscal |
| D2_QUANT | Quantidade Faturada | Quantidade faturada do item | Unidade depende do produto |
| D2_TOTAL | Valor Líquido do Item | Valor líquido do item faturado | Valor final da operação após descontos e ajustes |
| D2_VALBRUT | Valor Bruto | Valor bruto do item faturado | Valor antes de deduções |
| D_E_L_E_T_ | Exclusão Lógica | Indicador de exclusão lógica do ERP TOTVS | Registros deletados logicamente devem ser ignorados |

---

# Campos Relacionais

| Campo | Relacionamento | Entidade | Campo Entidade |
|---|---|---|---|
| D2_PEDIDO | pedido | SC5G10 | C5_NUM |
| D2_PEDIDO + D2_COD | item do pedido | SC6G10 | C6_NUM + C6_PRODUTO |
| D2_COD | produto | SB1G10 | B1_COD |
| D2_CLIENTE + D2_LOJA | cliente | SA1G10 | A1_COD + A1_LOJA |

---

# Regras Operacionais Identificadas

## Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

Registros deletados logicamente não deverão compor a camada analítica oficial.

---

## Produtos elegíveis para análise

```sql
LEN(D2_COD) = 6
AND (
    D2_COD LIKE '2M%'
    OR D2_COD LIKE 'PX%'
    OR D2_COD LIKE 'PI%'
    OR D2_COD LIKE '00%'
)
```

O modelo analítico atual considera apenas produtos dentro desse padrão operacional.

---

# Regras Semânticas Identificadas

## Natureza da entidade

A SD2 representa os itens efetivamente faturados/documentados fiscalmente.

A entidade possui natureza:
- fiscal
- comercial
- tributária
- financeira

---

## Relacionamento com pedidos

Um item do pedido poderá:
- não ser faturado
- ser faturado parcialmente
- gerar múltiplos faturamentos
- possuir faturamentos em datas diferentes

O relacionamento entre pedido e faturamento não é necessariamente 1:1.

---

## CFOP Fiscal

O campo:

```text
D2_CF
```

representa o CFOP efetivamente utilizado no documento fiscal.

Esse CFOP poderá divergir do CFOP originalmente informado no pedido comercial.

---

# Riscos Identificados

- múltiplos faturamentos para o mesmo pedido
- faturamentos parciais
- devoluções e ajustes fiscais
- divergência entre pedido e faturamento
- inconsistência tributária
- custos divergentes do custo contábil
- alterações fiscais posteriores

---

# Observações Importantes

A SD2 normalmente representa a principal origem para:
- faturamento
- receita bruta
- análises tributárias
- análises de margem

Os valores tributários poderão variar conforme:
- estado
- operação fiscal
- tipo de cliente
- classificação tributária

O campo:

```text
D2_CUSTO1
```

possui natureza operacional e poderá divergir:
- do custo contábil
- do custo médio
- do custo gerencial

O valor bruto do item:

```text
D2_VALBRUT
```

representa o valor bruto do item faturado antes de descontos e ajustes fiscais.

O campo:

```text
D2_TOTAL
```

representa o valor líquido efetivo da operação.

A definição oficial de faturamento deverá validar:
- impostos
- devoluções
- descontos
- cancelamentos
- regras fiscais

---

# Direcionamento Futuro

Os campos documentados nesta entidade deverão servir futuramente como origem para:
- fatos de faturamento
- análises tributárias
- análises de margem
- modelos MARTS
- regras fiscais centralizadas
- métricas comerciais

Estrutura prevista:

```text
RAW → STAGING → MARTS
```