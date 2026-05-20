# SA3G10 — Dicionário de Campos

## Objetivo

Documentar os principais campos operacionais da entidade SA3G10, responsável pelo cadastro de vendedores no ERP TOTVS.

O objetivo deste documento é centralizar:
- significado dos campos
- comportamento operacional
- observações importantes
- riscos conhecidos
- direcionamentos analíticos futuros

---

# Visão Geral

A entidade SA3G10 representa o cadastro de vendedores e responsáveis comerciais.

Cada registro representa um vendedor/responsável comercial utilizado nas operações de:
- pedidos de venda
- faturamento
- carteira comercial
- relatórios gerenciais

A entidade poderá conter:
- vendedores internos
- representantes
- executivos comerciais
- usuários inativos
- registros antigos

---

# Campos Operacionais

| Campo | Nome Semântico | Descrição | Observação |
|---|---|---|---|
| A3_COD | Código do Vendedor | Identificador operacional do vendedor | Chave operacional |
| A3_NOME | Nome do Vendedor | Nome completo do vendedor | Pode conter registros inativos/marcados |
| A3_NREDUZ | Nome Reduzido | Nome reduzido/apelido comercial do vendedor | Utilizado em relatórios e operações |
| D_E_L_E_T_ | Exclusão Lógica | Indicador de exclusão lógica do ERP TOTVS | Registros deletados logicamente devem ser ignorados |

---

# Campos Relacionais

| Campo | Relacionamento | Entidade | Campo Entidade |
|---|---|---|---|
| A3_COD | vendedor interno | SC5G10 | C5_VEND1 |
| A3_COD | representante | SC5G10 | C5_VEND2 |
| A3_COD | executivo | SC5G10 | C5_VENDE1 |

---

# Regras Operacionais Identificadas

## Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

Registros deletados logicamente não deverão compor a camada analítica oficial.

---

## Registros válidos

```sql
A3_NOME NOT LIKE '%*%'
```

Registros com nome contendo `"*"` deverão ser desconsiderados das análises oficiais.

---

# Regras Semânticas Identificadas

## Natureza da entidade

A SA3 representa o cadastro operacional/comercial de vendedores e responsáveis comerciais.

A entidade é utilizada para identificar responsáveis comerciais nas operações de venda e faturamento.

---

# Riscos Identificados

- vendedores inativos ainda vinculados a pedidos antigos
- registros antigos não removidos fisicamente
- duplicidade de vendedores
- alterações manuais de cadastro
- utilização inconsistente entre vendedor interno e representante
- nomes divergentes ao longo do tempo

---

# Observações Importantes

O mesmo vendedor poderá assumir diferentes papéis operacionais:
- executivo
- vendedor interno
- representante

Nem todos os registros da SA3 representam vendedores ativos.

Registros antigos poderão continuar aparecendo em operações históricas.

O campo:

```text
A3_NREDUZ
```

normalmente é utilizado em relatórios operacionais e visuais analíticos devido ao nome reduzido.

---

# Direcionamento Futuro

Os campos documentados nesta entidade deverão servir futuramente como origem para:
- dimensões comerciais
- análises de vendedores
- regras centralizadas no dbt
- modelos MARTS
- indicadores comerciais

Estrutura prevista:

```text
RAW → STAGING → MARTS
```