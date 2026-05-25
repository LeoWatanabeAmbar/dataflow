# Valor Bruto da Venda

## Objetivo

Definir a regra oficial de cálculo do valor bruto comercial dos pedidos de venda, considerando:

- valor operacional do item
- frete proporcional
- IPI
- ICMS ST

---

# Conceito

O valor bruto comercial representa o valor efetivo da venda utilizado nas análises comerciais da empresa.

O cálculo é realizado a partir da base operacional de pedidos:

```text
SC6G10
```

com enriquecimento fiscal e comercial proveniente de:
- SC5
- SB1
- SF4
- SF7
- SA1

---

# Camada responsável

Esta regra pertence à camada:

```text
INTERMEDIATE
```

---

# Fontes utilizadas

## Itens do pedido

```text
SC6G10
```

Campos:
- C6_NUM
- C6_PRODUTO
- C6_QTDVEN
- C6_QTDENT
- C6_VALOR
- C6_TES
- C6_CF

---

## Pedido

```text
SC5G10
```

Campos:
- C5_FRETE
- C5_TIPOCLI
- C5_ESTE
- C5_ZZMODF2

---

## Produto

```text
SB1G10
```

Campos:
- B1_IPI
- B1_GRTRIB
- B1_CEST

---

## Cliente

```text
SA1G10
```

Campos:
- A1_EST
- A1_GRPTRIB
- A1_CONTRIB

---

## TES

```text
SF4G10
```

Campos:
- F4_ICM
- F4_MKPCMP

---

## Regra tributária

```text
SF7G10
```

Campos:
- F7_GRTRIB
- F7_EST
- F7_TIPOCLI
- F7_GRPCLI
- F7_MARGEM

---

# Regra oficial

## Fórmula

```text
Valor Bruto Total =
    C6_VALOR
    + Valor Frete Item
    + Valor IPI Item
    + ICMS-ST
```

---

# Componentes do cálculo

## 1. Valor operacional

Origem:

```text
SC6.C6_VALOR
```

Representa:
- valor operacional do item
- já contendo impostos operacionais embutidos pelo ERP

---

## 2. Frete proporcional

O frete é distribuído proporcionalmente sobre o valor do item.

### Fórmula

```text
Aliquota Frete =
    C5_FRETE / C5_VALOR
```

```text
Valor Frete Item =
    Aliquota Frete × C6_VALOR
```

---

## 3. IPI

O IPI é calculado sobre:
- valor do item
- frete proporcional

### Fórmula

```text
Aliquota IPI =
    B1_IPI / 100
```

```text
Valor IPI Item =
    Aliquota IPI × (C6_VALOR + Valor Frete Item)
```

---

# ICMS ST

## Objetivo

Calcular o impacto de substituição tributária no valor bruto comercial.

---

## Critério de elegibilidade

O item é considerado ST quando:

```text
(F4_MKPCMP = '2' AND C5_TIPOCLI = 'S')
```

ou

```text
(C6_CF IN ('6401','6403')
AND A1_CONTRIB = '1'
AND A1_GRPTRIB <> 'SOL')
```

---

## Base de cálculo

### Valor com IPI

```text
Valor com IPI =
    C6_VALOR + Valor IPI Item
```

---

## ICMS interno

```text
ICMS Interno =
    (
        (Valor com IPI × F7_MARGEM)
        + Valor com IPI
    ) × 0.18
```

---

## ICMS interestadual

```text
ICMS Interestadual =
    Valor com IPI × ICMS_INTER
```

---

## ICMS ST final

```text
ICMS-ST =
    ICMS Interno - ICMS Interestadual
```

---

# Quantidade não faturada

## Fórmula

```text
Quantidade Não Faturada =
    C6_QTDVEN - C6_QTDENT
```

---

# Valor bruto não faturado

## Fórmula

```text
Valor Bruto Não Faturado =
    (Valor Bruto Total / C6_QTDVEN)
    × Quantidade Não Faturada
```

---

# Triangulação

Pedidos classificados como triangulação:
- duplicam registros
- redistribuem representantes
- dividem quantidades e valores

A identificação ocorre quando:

```text
C5_ZZMODF2 contém "REMESSA"
```

---

# Regra da triangulação

Os seguintes campos são divididos pela metade:

- Valor Bruto Total
- Quantidade Não Faturada
- Valor Bruto Não Faturado
- C6_QTDVEN
- C6_QTDENT
- C6_VALOR
- Valor Frete Item
- Valor IPI Item
- ICMS-ST

---

# Regras técnicas

## Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

---

## Bloqueio operacional

```sql
C6_BLQ <> 'R'
```

---

## Produtos elegíveis

```text
LEN(C6_PRODUTO) = 6
```

Excluindo:
- A_____
- E_____

---

# Estrutura sugerida

## INTERMEDIATE

Tabela sugerida:

```text
int_venda_valor_bruto
```

---

# Campos gerados

| Campo | Descrição |
|---|---|
| valor_operacional | Valor operacional do item |
| valor_frete_item | Frete proporcional |
| valor_ipi_item | IPI do item |
| valor_icms_st | Valor de ICMS ST |
| valor_bruto_total | Valor bruto final |
| valor_bruto_nao_faturado | Carteira não faturada |

---

# Impacto nas Métricas

Esta regra alimenta:

- faturamento bruto
- venda bruta
- carteira
- programação
- margem
- análises comerciais

---

# Resultado esperado

Após aplicação desta regra:

- o valor bruto comercial fica padronizado
- triangulação fica consistente
- carteira passa a refletir valor real
- elimina divergências entre relatórios
- reduz cálculos manuais no Power BI