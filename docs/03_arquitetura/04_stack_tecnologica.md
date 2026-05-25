# Stack Tecnológica

## Objetivo

Definir as tecnologias utilizadas em cada camada da arquitetura analítica, garantindo clareza sobre responsabilidades técnicas e separação de ferramentas.

---

# Visão Geral da Stack

```text
ERP TOTVS (SQL Server)
        ↓
EXTRAÇÃO / INTEGRAÇÃO
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
POWER BI
```

---

# 1. ERP / Fonte de Dados

## Tecnologia

- TOTVS Protheus
- SQL Server

## Responsabilidade

- armazenamento operacional
- transações comerciais
- faturamento
- cadastro

---

# 2. Extração de Dados

## Tecnologia atual

- SQL (queries diretas)
- Power Query (temporário)

## Futuro recomendado

- dbt + scheduler
- pipelines SQL estruturados

## Responsabilidade

- copiar dados do ERP para camada RAW
- manter histórico consistente

---

# 3. RAW

## Tecnologia

- SQL Server (replicação)
- ou PostgreSQL (futuro)

## Responsabilidade

- armazenamento bruto
- espelhamento do ERP
- sem regras de negócio

---

# 4. STAGING

## Tecnologia

- SQL
- dbt (recomendado)

## Responsabilidade

- padronização técnica
- limpeza de dados
- tipos e formatos
- exclusão lógica

---

# 5. INTERMEDIATE

## Tecnologia principal

- dbt (camada recomendada)

## Responsabilidade

- regras de negócio
- transformação semântica
- cálculos intermediários

## Regras aplicadas aqui

- triangulação
- programação comercial
- CFOP
- grupo comercial
- produtos elegíveis

---

# 6. MARTS

## Tecnologia

- dbt
- SQL (views ou tabelas materializadas)

## Responsabilidade

- modelagem dimensional
- fatos e dimensões
- estrutura de consumo

---

## Estruturas principais

- fct_pedido_item
- fct_faturamento_item
- dim_cliente
- dim_produto
- dim_vendedor
- dim_grupo_comercial

---

# 7. SEMANTIC LAYER

## Tecnologia

- Power BI (modelos semânticos)
- ou ferramenta futura (Fabric / Looker / etc)

## Responsabilidade

- KPIs oficiais
- métricas corporativas
- padronização de cálculos

---

# 8. POWER BI (Consumo)

## Tecnologia

- Power BI Desktop / Service

## Responsabilidade

- visualização
- análise
- dashboards

## O que NÃO deve existir

- regras de negócio
- transformação de dados
- cálculos complexos duplicados

---

# Ferramentas Recomendadas (Visão Futuro)

## Transformação

- dbt (principal recomendação)

---

## Orquestração

- Airflow (opcional)
- GitHub Actions (simples)
- cron jobs

---

## Banco Analítico

- PostgreSQL
- Supabase
- ou SQL Server (transição gradual)

---

## Versionamento

- GitHub

---

# Princípio da Stack

```text
Cada ferramenta deve ter uma responsabilidade única
```

---

# Resultado final esperado

Com essa stack, o projeto garante:

- separação de responsabilidades
- escalabilidade
- governança
- rastreabilidade
- redução de lógica no Power BI