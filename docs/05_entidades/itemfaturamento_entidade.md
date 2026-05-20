# Entidade — fct_faturamento_item

## Objetivo

Consolidar os itens faturados da operação comercial/fiscal da empresa.

A entidade tem como objetivo centralizar:
- faturamento
- valores fiscais
- custos
- impostos
- quantidades faturadas
- informações comerciais

A fato será utilizada como principal origem para análises:
- comerciais
- faturamento
- margem
- tributos
- rentabilidade
- indicadores gerenciais

---

# Granularidade

Cada registro representa:

```text
1 item faturado
```

A granularidade da entidade corresponde ao item presente no documento fiscal de saída.

---

# Origens

| Entidade | Objetivo |
|---|---|
| SD2G10 | Itens faturados/documentos fiscais |
| SA1G10 | Clientes |
| SB1G10 | Produtos |
| SA3G10 | Vendedores |
| SBMG10 | Grupo comercial/econômico |

---

# Relacionamentos Principais

| Entidade Relacionada | Tipo |
|---|---|
| dim_cliente | cliente faturado |
| dim_produto | produto faturado |
| dim_vendedor | responsável comercial |
| dim_grupo_comercial | agrupamento econômico/comercial |

---

# Principais Atributos

| Atributo | Origem | Descrição |
|---|---|---|
| numero_nota | SD2.D2_DOC | Número do documento fiscal |
| numero_pedido | SD2.D2_PEDIDO | Pedido vinculado ao faturamento |
| data_faturamento | SD2.D2_EMISSAO | Data de emissão fiscal |
| codigo_produto | SD2.D2_COD | Produto faturado |
| codigo_cliente | SD2.D2_CLIENTE | Cliente faturado |
| loja_cliente | SD2.D2_LOJA | Loja/unidade do cliente |
| cfop | SD2.D2_CF | Código fiscal da operação |

---

# Principais Métricas

| Métrica | Origem | Descrição |
|---|---|---|
| quantidade_faturada | SD2.D2_QUANT | Quantidade faturada |
| valor_bruto | SD2.D2_VALBRUT | Valor bruto do item |
| valor_liquido | SD2.D2_TOTAL | Valor líquido do item |
| custo_item | SD2.D2_CUSTO1 | Custo operacional do item |
| valor_icms | SD2.D2_VALICM | Valor de ICMS |
| valor_ipi | SD2.D2_VALIPI | Valor de IPI |
| valor_difal | SD2.D2_DIFAL | Valor de DIFAL |
| valor_pis | SD2.D2_VALIMP5 | Valor de PIS |
| valor_cofins | SD2.D2_VALIMP6 | Valor de COFINS |
| valor_frete | SD2.D2_VALFRE | Valor de frete |
| valor_icms_retido | SD2.D2_ICMSRET | Valor de ICMS retido/ST |

---

# Regras Operacionais

## Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

Registros deletados logicamente não deverão compor a fato oficial.

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

A camada analítica considera apenas produtos dentro desse padrão operacional.

---

# Regras Semânticas

## Natureza da entidade

A entidade representa os itens efetivamente faturados/documentados fiscalmente.

A fato possui natureza:
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

## Valor bruto e líquido

O campo:

```text
D2_VALBRUT
```

representa o valor bruto do item faturado antes de descontos e ajustes.

O campo:

```text
D2_TOTAL
```

representa o valor líquido efetivo do item faturado.

---

## CFOP fiscal

O CFOP presente na SD2 representa o CFOP efetivamente utilizado no documento fiscal.

Esse CFOP poderá divergir:
- do CFOP do pedido
- da operação comercial original

---

# Métricas Derivadas Futuras

A entidade poderá futuramente compor:
- margem bruta
- margem líquida
- ticket médio
- rentabilidade
- faturamento líquido
- faturamento bruto
- indicadores fiscais
- análises tributárias

---

# Riscos Identificados

- múltiplos faturamentos para o mesmo pedido
- faturamentos parciais
- devoluções
- cancelamentos
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
- classificação tributária
- tipo de cliente

O custo operacional poderá divergir:
- do custo contábil
- do custo médio do estoque
- do custo gerencial oficial

A definição oficial de faturamento deverá considerar:
- cancelamentos
- devoluções
- impostos
- descontos
- regras fiscais

---

# Direcionamento Futuro

A entidade deverá futuramente servir como base para:
- camada MARTS
- dashboards comerciais
- análises financeiras
- indicadores estratégicos
- análises tributárias
- modelos semânticos
- regras centralizadas no dbt

Estrutura prevista:

```text
RAW → STAGING → FCT_FATURAMENTO_ITEM
```