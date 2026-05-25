# Regra de Negócio — Triangulação

## Objetivo

Documentar as regras operacionais e analíticas utilizadas no tratamento de operações de triangulação comercial/fiscal.

A regra tem como objetivo:
- evitar duplicidade de faturamento
- representar corretamente os responsáveis comerciais
- controlar impacto de remessas
- padronizar indicadores comerciais
- garantir consistência analítica

---

# Contexto de Negócio

Determinadas operações comerciais da empresa possuem natureza de:

```text
TRIANGULAÇÃO
```

Nessas operações:
- existe uma movimentação de remessa
- existe uma venda comercial vinculada
- existem múltiplos responsáveis comerciais
- a operação logística/fiscal difere da operação comercial

---

# Identificação da Triangulação

A triangulação é identificada através do campo:

```text
MODFAT
```

Critério operacional atual:

```text
MODFAT contém "REMESSA"
```

ou

```text
MODFAT contém "VENDA"
```

---

# Comportamento Analítico

## Duplicação Analítica

Pedidos triangulados são duplicados analiticamente para permitir representação dos responsáveis comerciais.

A operação gera:
- uma linha para o representante
- uma linha para o executivo

---

# Responsáveis Comerciais

## Representante

A linha original da operação permanece vinculada ao:

```text
Representante
```

---

## Executivo

Uma segunda linha analítica é criada vinculando a operação ao:

```text
Executivo
```

---

# Regra de Rateio

## Rateio Comercial

As métricas comerciais são rateadas entre os responsáveis da operação.

Rateio atual:

```text
50% Representante
50% Executivo
```

---

# Métricas Rateadas

As seguintes métricas possuem rateio:

- faturamento bruto
- quantidade faturada
- ICMS
- IPI
- PIS
- COFINS
- DIFAL
- frete
- ICMS ST

---

# Tratamento da Remessa

## Neutralização Comercial

Registros classificados como:

```text
Triangulação REMESSA
```

não deverão compor:
- faturamento comercial
- quantidade comercial
- impostos comerciais

Nesses casos:
- os valores comerciais são zerados
- a operação permanece apenas para representação operacional/logística

---

# Tratamento do Custo

## Custos Operacionais

O custo operacional poderá permanecer parcialmente associado à remessa.

Objetivos:
- preservar rastreabilidade operacional
- representar impacto logístico
- manter coerência de custos

---

# Impactos Analíticos

A regra impacta diretamente:
- faturamento
- margem
- comissão
- indicadores comerciais
- performance de vendedores
- rentabilidade
- dashboards gerenciais

---

# Riscos Identificados

- duplicidade de faturamento
- superavaliação de receita
- distorção de margem
- divergência entre operação fiscal e comercial
- inconsistência de responsáveis comerciais
- alterações manuais no campo MODFAT

---

# Observações Importantes

A triangulação possui natureza:
- comercial
- fiscal
- logística

A interpretação analítica da operação não deverá considerar apenas:
- pedidos
- notas fiscais
- movimentações fiscais

mas também:
- regras comerciais corporativas
- responsáveis comerciais
- política de rateio

---

# Direcionamento Técnico Futuro

A regra deverá futuramente ser centralizada em:
- modelos dbt
- camada semântica corporativa
- modelos intermediários
- regras oficiais de métricas

Estrutura prevista:

```text
RAW → STAGING → INTERMEDIATE → MARTS
```
 
Possível implementação futura:

```text
int_triangulacao
```

ou

```text
int_faturamento_triangulacao
```

---

# Entidades Impactadas

A regra impacta diretamente:
- fct_pedido_item
- fct_faturamento_item
- métricas comerciais
- dashboards gerenciais
- indicadores financeiros/comerciais