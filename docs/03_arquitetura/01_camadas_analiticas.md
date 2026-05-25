# Camadas Analíticas

## Objetivo

Descrever em detalhe a responsabilidade de cada camada da arquitetura analítica e as regras aplicadas em cada etapa do pipeline de dados.

---

# Visão Geral das Camadas

```text
RAW → STAGING → INTERMEDIATE → MARTS → SEMANTIC LAYER
```

Cada camada possui uma responsabilidade única e não deve conter lógica fora do seu escopo.

---

# RAW

## Objetivo

Armazenar os dados exatamente como vêm do ERP TOTVS.

---

## Características

- dados brutos
- sem tratamento
- sem regras de negócio
- sem joins analíticos
- sem agregações
- preservação total da origem

---

## Responsabilidade

A camada RAW serve como:
- histórico técnico
- fonte única de verdade
- base para auditoria

---

## Exemplos de tabelas

- SC5 (Pedidos)
- SC6 (Itens de pedido)
- SD2 (Faturamento)
- SA1 (Clientes)
- SA3 (Vendedores)
- SB1 (Produtos)
- SBM (Grupos comerciais)

---

## Regras permitidas

Apenas:
- carga incremental
- replicação fiel
- controle de duplicidade técnica

---

# STAGING

## Objetivo

Padronizar tecnicamente os dados para uso analítico.

---

## Responsabilidades

- padronização de tipos
- limpeza de texto (TRIM, UPPER, etc.)
- remoção de registros excluídos logicamente
- renomeação de colunas
- filtros técnicos básicos

---

## Regras aplicadas

### Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

---

### Normalização de campos

- datas padronizadas
- textos tratados
- números convertidos

---

## O que NÃO deve existir

- regras de negócio
- cálculo de faturamento
- classificação comercial
- lógica de triangulação
- regras fiscais

---

# INTERMEDIATE

## Objetivo

Centralizar todas as regras de negócio corporativas.

---

## Características

Esta é a camada mais importante da arquitetura.

Aqui os dados deixam de ser técnicos e passam a ser semânticos.

---

## Responsabilidades

- aplicação de regras de negócio
- criação de campos derivados
- enriquecimento de dados
- consolidação de entidades

---

## Regras implementadas nesta camada

### Triangulação

- identificação via SC5.C5_ZZMODF2
- duplicação de registros
- rateio entre representantes
- separação comercial vs remessa

---

### Programação comercial

- extração de datas do campo textual
- classificação em:
  - Programado
  - Carteira
- definição de horizonte temporal

---

### CFOP

- classificação da natureza da operação:
  - venda
  - remessa
  - devolução
  - transferência
  - bonificação

---

### Produtos elegíveis

- filtro por padrão de código
- exclusão de produtos inválidos
- padronização de famílias comerciais

---

### Grupos comerciais

- agrupamento de clientes via SBM / SA1.A1_TELEX
- consolidação de carteira por grupo econômico

---

## O que NÃO deve existir

- visualizações
- medidas de BI
- regras específicas de dashboard
- lógica de consumo final

---

# MARTS

## Objetivo

Criar as entidades analíticas finais para consumo.

---

## Características

Camada orientada a:
- performance
- consumo
- modelagem dimensional

---

## Tipos de tabelas

### Dimensões

- dim_cliente
- dim_produto
- dim_vendedor
- dim_grupo_comercial
- dim_cfop
- dim_tempo

---

### Fatos

- fct_pedido_item
- fct_faturamento_item

---

## Responsabilidade

- modelagem estrela
- joins consolidados
- estrutura pronta para BI

---

## O que NÃO deve existir

- regras de negócio complexas
- parsing de campos
- lógica de triangulação
- cálculos não padronizados

---

# SEMANTIC LAYER

## Objetivo

Definir as métricas oficiais da empresa.

---

## Características

Camada mais próxima do negócio final.

---

## Responsabilidades

- KPIs oficiais
- padronização de métricas
- definição única de cálculos
- governança de indicadores

---

## Exemplos de métricas

- faturamento bruto
- faturamento líquido
- margem
- carteira
- forecast
- ticket médio
- performance de vendedor

---

## Regras

- nenhuma lógica complexa deve existir aqui
- apenas agregações e definições oficiais
- consistência obrigatória entre relatórios

---

# Princípios Gerais da Arquitetura

## 1. Separação de responsabilidades

Cada camada tem um único propósito.

---

## 2. Centralização de regras

Toda regra de negócio deve existir apenas no INTERMEDIATE.

---

## 3. Reutilização

Transformações devem ser reutilizáveis entre diferentes análises.

---

## 4. Rastreabilidade

Todo dado deve ser rastreável até sua origem no ERP.

---

## 5. Consistência

O mesmo indicador deve ter o mesmo valor em qualquer relatório.

---

# Resultado esperado da arquitetura

Com essa estrutura, o projeto passa a ter:

- governança de dados
- métricas consistentes
- regras centralizadas
- redução de dependência do Power BI
- escalabilidade analítica
- maior confiabilidade dos dados