# Arquitetura Geral

## Objetivo

Definir a arquitetura analítica utilizada no projeto de BI corporativo.

A arquitetura possui como objetivo:
- centralizar regras de negócio
- padronizar métricas
- garantir governança analítica
- desacoplar regras do Power BI
- estruturar pipeline de dados escalável
- consolidar camada semântica corporativa

---

# Visão Geral

Fluxo macro da arquitetura:

```text
ERP TOTVS
    ↓
RAW
    ↓
STAGING
    ↓
INTERMEDIATE
    ↓
MARTS
    ↓
SEMANTIC LAYER
    ↓
Power BI
```

---

# Fonte de Dados

Sistema principal:

```text
TOTVS Protheus
```

Banco operacional atual:

```text
SQL Server
```

---

# Principais Entidades Operacionais

| Entidade | Descrição |
|---|---|
| SC5 | Pedidos de venda |
| SC6 | Itens do pedido |
| SD2 | Itens faturados |
| SA1 | Clientes |
| SA3 | Vendedores |
| SB1 | Produtos |
| SBM | Grupo comercial |

---

# Objetivos da Arquitetura

A arquitetura busca separar:
- dados operacionais
- transformações técnicas
- regras de negócio
- entidades analíticas
- métricas corporativas
- visualização

---

# Princípios da Arquitetura

## Separação de Responsabilidades

Cada camada deverá possuir responsabilidade específica e independente.

---

## Centralização de Regras

Regras de negócio deverão ser centralizadas fora do Power BI.

Objetivo:
- evitar duplicidade de lógica
- reduzir inconsistências
- aumentar rastreabilidade

---

## Governança Analítica

Indicadores oficiais deverão possuir:
- definição formal
- documentação
- rastreabilidade
- consistência corporativa

---

## Reutilização

Transformações deverão ser reutilizáveis entre:
- dashboards
- relatórios
- métricas
- análises
- modelos analíticos

---

## Escalabilidade

A arquitetura deverá permitir:
- crescimento de entidades
- inclusão de novas regras
- aumento de volume de dados
- expansão analítica futura

---

# Estrutura das Camadas

| Camada | Objetivo |
|---|---|
| RAW | Armazenamento bruto dos dados operacionais |
| STAGING | Limpeza e padronização técnica |
| INTERMEDIATE | Aplicação de regras de negócio |
| MARTS | Modelagem analítica final |
| SEMANTIC LAYER | Métricas e indicadores oficiais |

---

# Camada RAW

Responsável por:
- replicação dos dados operacionais
- armazenamento bruto
- preservação da origem

Características:
- sem regras de negócio
- sem joins analíticos
- sem agregações

---

# Camada STAGING

Responsável por:
- padronização técnica
- tipagem
- trims
- filtros básicos
- exclusão lógica
- renomeação de campos

Exemplos:
- remoção de registros deletados
- tratamento de tipos
- normalização de textos

---

# Camada INTERMEDIATE

Responsável por:
- centralização das regras de negócio
- enriquecimento de dados
- tratamentos operacionais complexos
- consolidação semântica

Exemplos:
- triangulação
- classificação CFOP
- programação comercial
- produtos elegíveis
- regras comerciais

---

# Camada MARTS

Responsável por:
- entidades analíticas finais
- tabelas fato
- dimensões
- consumo analítico

Exemplos:
- dim_cliente
- dim_produto
- dim_vendedor
- dim_grupo_comercial
- fct_pedido_item
- fct_faturamento_item

---

# Semantic Layer

Responsável por:
- métricas oficiais
- KPIs corporativos
- nomenclaturas padronizadas
- indicadores consolidados

Exemplos:
- faturamento líquido
- margem
- carteira
- forecast
- ticket médio

---

# Tecnologias Planejadas

| Camada | Tecnologia |
|---|---|
| ERP | TOTVS Protheus |
| Banco operacional | SQL Server |
| Transformação | dbt |
| Banco analítico | PostgreSQL / Supabase |
| Visualização | Power BI |

---

# Estrutura Prevista do Projeto

```text
/docs
    /arquitetura
    /entidades
    /regras_negocio

/models
    /raw
    /staging
    /intermediate
    /marts

/seeds

/tests

/macros
```

---

# Direcionamento Futuro

A arquitetura deverá evoluir para:
- centralização semântica
- versionamento de regras
- automação de pipelines
- governança corporativa
- maior rastreabilidade
- desacoplamento completo do Power BI
- escalabilidade analítica