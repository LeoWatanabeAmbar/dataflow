# Entidade — dim_grupo_comercial

## Objetivo

Consolidar os grupos comerciais/econômicos utilizados para agrupamento analítico de clientes da empresa.

A entidade tem como objetivo centralizar:
- agrupamentos comerciais
- grupos econômicos
- consolidação de clientes
- consolidação de obras/unidades
- relacionamento corporativo

A dimensão será utilizada como base para análises:
- faturamento por grupo
- concentração de clientes
- carteira corporativa
- participação comercial
- indicadores estratégicos

---

# Granularidade

Cada registro representa:

```text
1 grupo comercial
```

Chave operacional:

```text
BM_GRUPO
```

---

# Origens

| Entidade | Objetivo |
|---|---|
| SBMG10 | Cadastro de grupos comerciais |
| SA1G10 | Relacionamento cliente → grupo |

---

# Relacionamentos Principais

| Entidade Relacionada | Tipo |
|---|---|
| dim_cliente | agrupamento comercial do cliente |
| fct_pedido_item | consolidação comercial |
| fct_faturamento_item | consolidação de faturamento |

---

# Principais Atributos

| Atributo | Origem | Descrição |
|---|---|---|
| codigo_grupo_comercial | SBM.BM_GRUPO | Código operacional do grupo |
| grupo_comercial | SBM.BM_DESC | Nome/descrição do grupo comercial |

---

# Regras Operacionais

## Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

Registros deletados logicamente não deverão compor a dimensão oficial.

---

# Regras Semânticas

## Natureza da entidade

A dimensão representa agrupamentos comerciais/econômicos utilizados pela operação comercial da empresa.

O agrupamento permite consolidar diferentes:
- clientes
- lojas
- obras
- filiais
- CNPJs

em uma única visão analítica.

---

## Relacionamento com clientes

Relacionamento operacional atual:

```text
SA1.A1_TELEX = SBM.BM_GRUPO
```

O campo:

```text
A1_TELEX
```

é utilizado operacionalmente como identificador do grupo comercial do cliente.

---

## Exemplos de grupos comerciais

Exemplos atuais:
- MRV
- TENDA
- DIRECIONAL

Um mesmo grupo poderá possuir:
- múltiplos clientes
- múltiplos CNPJs
- múltiplas obras
- múltiplas unidades comerciais

---

# Métricas Relacionadas

A entidade será utilizada para composição de:
- faturamento por grupo econômico
- margem por grupo
- participação comercial
- concentração de receita
- carteira corporativa
- volume vendido
- ticket médio corporativo

---

# Riscos Identificados

- agrupamentos incorretos
- clientes vinculados ao grupo errado
- ausência de padronização operacional
- alterações manuais no cadastro
- dependência de preenchimento operacional
- grupos comerciais desatualizados

---

# Observações Importantes

O agrupamento comercial possui natureza:
- operacional
- comercial
- analítica

O grupo comercial não representa necessariamente:
- vínculo societário formal
- holding oficial
- estrutura jurídica corporativa

A qualidade analítica da entidade depende diretamente:
- da manutenção do cadastro de clientes
- da consistência do campo A1_TELEX
- da governança operacional

---

# Direcionamento Futuro

A entidade deverá futuramente servir como base para:
- camada MARTS
- segmentação comercial
- indicadores estratégicos
- análises corporativas
- modelos semânticos
- regras centralizadas no dbt

Estrutura prevista:

```text
RAW → STAGING → DIM_GRUPO_COMERCIAL
```