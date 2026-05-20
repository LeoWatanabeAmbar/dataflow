# SBMG10 — Dicionário de Campos

## Objetivo

Documentar os principais campos operacionais da entidade SBMG10, responsável pelo cadastro de grupos comerciais/econômicos utilizados nas análises comerciais do ERP TOTVS.

O objetivo deste documento é centralizar:
- significado dos campos
- comportamento operacional
- observações importantes
- riscos conhecidos
- direcionamentos analíticos futuros

---

# Visão Geral

A entidade SBMG10 representa o cadastro de grupos comerciais/econômicos utilizados para consolidação analítica de clientes.

Cada registro representa um grupo comercial.

A entidade é utilizada para consolidar:
- clientes
- obras
- filiais
- múltiplos CNPJs

em um único agrupamento comercial/econômico.

O relacionamento principal ocorre através do campo:

```text
SA1.A1_TELEX = SBMG10.BM_GRUPO
```

---

# Campos Operacionais

| Campo | Nome Semântico | Descrição | Observação |
|---|---|---|---|
| BM_GRUPO | Código do Grupo Comercial | Identificador do grupo comercial/econômico | Chave operacional do agrupamento |
| BM_DESC | Descrição do Grupo Comercial | Nome do grupo comercial/econômico | Utilizado em análises e dashboards |
| D_E_L_E_T_ | Exclusão Lógica | Indicador de exclusão lógica do ERP TOTVS | Registros deletados logicamente devem ser ignorados |

---

# Campos Relacionais

| Campo | Relacionamento | Entidade | Campo Entidade |
|---|---|---|---|
| BM_GRUPO | agrupamento comercial do cliente | SA1G10 | A1_TELEX |

---

# Regras Operacionais Identificadas

## Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

Registros deletados logicamente não deverão compor a camada analítica oficial.

---

# Regras Semânticas Identificadas

## Natureza da entidade

A SBMG10 representa uma tabela auxiliar de agrupamento comercial/econômico.

A entidade é utilizada para consolidar clientes em grupos estratégicos de análise comercial.

---

## Agrupamento Econômico

O agrupamento comercial permite consolidar diferentes:
- clientes
- lojas
- obras
- CNPJs

em uma única visão comercial.

Exemplos:

| Código | Grupo Comercial |
|---|---|
| C02 | MRV |
| C03 | TENDA |

Esse agrupamento é utilizado principalmente para:
- consolidação de faturamento
- visão estratégica de clientes
- carteira comercial
- análises gerenciais
- indicadores comerciais

---

# Riscos Identificados

- agrupamentos preenchidos manualmente
- ausência de padronização operacional
- clientes vinculados ao grupo incorreto
- grupos econômicos desatualizados
- dependência de manutenção manual

---

# Observações Importantes

O agrupamento comercial não representa necessariamente vínculo societário formal entre empresas.

Um mesmo grupo poderá possuir:
- múltiplos clientes
- múltiplas lojas
- múltiplos CNPJs
- múltiplas obras

A qualidade analítica desse agrupamento depende diretamente da manutenção operacional dos cadastros.

---

# Direcionamento Futuro

Os campos documentados nesta entidade deverão servir futuramente como origem para:
- dimensões de grupos econômicos
- análises estratégicas de clientes
- modelos MARTS
- consolidação comercial
- regras centralizadas no dbt

Estrutura prevista:

```text
RAW → STAGING → MARTS
```