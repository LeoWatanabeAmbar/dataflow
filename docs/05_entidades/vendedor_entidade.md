# Entidade — dim_vendedor

## Objetivo

Consolidar as informações dos vendedores e responsáveis comerciais utilizados nas operações da empresa.

A entidade tem como objetivo centralizar:
- identificação dos vendedores
- responsáveis comerciais
- representantes
- executivos
- relacionamento comercial

A dimensão será utilizada como base para análises:
- comerciais
- faturamento
- carteira de clientes
- desempenho de vendedores
- indicadores gerenciais

---

# Granularidade

Cada registro representa:

```text
1 vendedor
```

Chave operacional:

```text
A3_COD
```

---

# Origens

| Entidade | Objetivo |
|---|---|
| SA3G10 | Cadastro principal de vendedores |

---

# Relacionamentos Principais

| Entidade Relacionada | Tipo |
|---|---|
| fct_pedido_item | vendedor do pedido |
| fct_faturamento_item | vendedor do faturamento |

---

# Principais Atributos

| Atributo | Origem | Descrição |
|---|---|---|
| codigo_vendedor | SA3.A3_COD | Código operacional do vendedor |
| vendedor | SA3.A3_NOME | Nome completo do vendedor |
| vendedor_reduzido | SA3.A3_NREDUZ | Nome reduzido/apelido comercial |

---

# Regras Operacionais

## Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

Registros deletados logicamente não deverão compor a dimensão oficial.

---

## Registros válidos

```sql
A3_NOME NOT LIKE '%*%'
```

Registros com nome contendo `"*"` deverão ser desconsiderados das análises oficiais.

---

# Regras Semânticas

## Natureza da entidade

A dimensão representa os responsáveis comerciais utilizados nas operações da empresa.

A entidade poderá representar:
- vendedores internos
- representantes comerciais
- executivos comerciais

---

## Utilização operacional

O mesmo vendedor poderá assumir diferentes papéis operacionais:
- executivo
- vendedor interno
- representante

Dependendo do contexto operacional, o relacionamento poderá ocorrer através de:
- C5_VENDE1
- C5_VEND1
- C5_VEND2

---

# Métricas Relacionadas

A entidade será utilizada para composição de:
- faturamento por vendedor
- margem por vendedor
- carteira de clientes
- volume vendido
- desempenho comercial
- ticket médio
- indicadores gerenciais

---

# Riscos Identificados

- vendedores inativos ainda vinculados a operações históricas
- duplicidade de vendedores
- alterações manuais de cadastro
- utilização inconsistente entre vendedor interno e representante
- nomes divergentes ao longo do tempo

---

# Observações Importantes

Nem todos os registros da SA3 representam vendedores ativos.

Registros antigos poderão continuar vinculados:
- a pedidos
- faturamentos
- operações históricas

O campo:

```text
A3_NREDUZ
```

normalmente é utilizado em:
- dashboards
- relatórios operacionais
- indicadores gerenciais

devido ao nome reduzido.

---

# Direcionamento Futuro

A entidade deverá futuramente servir como base para:
- camada MARTS
- análises comerciais
- indicadores de desempenho
- segmentações comerciais
- modelos semânticos
- regras centralizadas no dbt

Estrutura prevista:

```text
RAW → STAGING → DIM_VENDEDOR
```