# Fluxo de Dados

## Objetivo

Descrever o fluxo completo de movimentação dos dados desde o ERP TOTVS até a camada de consumo analítico no Power BI.

---

# Visão Geral do Fluxo

```text
ERP TOTVS
    ↓
EXTRAÇÃO
    ↓
RAW
    ↓
STAGING
    ↓
INTERMEDIATE (regras de negócio)
    ↓
MARTS (modelagem dimensional)
    ↓
SEMANTIC LAYER (KPIs oficiais)
    ↓
POWER BI (consumo)
```

---

# 1. Origem dos Dados (ERP TOTVS)

Os dados são originados no sistema:

- Protheus TOTVS

Principais módulos:
- vendas
- faturamento
- cadastro
- produtos
- financeiro

---

# 2. Extração

## Objetivo

Extrair os dados do ERP para camada RAW.

## Características

- replicação fiel
- sem transformações de negócio
- carga incremental ou full

---

# 3. RAW

## Objetivo

Armazenamento bruto dos dados.

## O que acontece aqui

- preservação total do dado original
- nenhuma regra aplicada
- nenhum join analítico

---

# 4. STAGING

## Objetivo

Padronização técnica dos dados.

## Processos

- limpeza de dados
- normalização de textos
- conversão de tipos
- remoção de registros excluídos
- ajustes técnicos

---

# 5. INTERMEDIATE (Camada de Regras)

## Objetivo

Aplicação de regras de negócio corporativas.

---

## Regras aplicadas nesta etapa

### Triangulação
- identificação via C5_ZZMODF2
- duplicação de registros
- rateio entre responsáveis comerciais
- separação venda vs remessa

---

### Programação comercial
- extração de datas de texto
- classificação:
  - Programado
  - Carteira
- definição de horizonte de previsão

---

### CFOP
- classificação fiscal da operação
- definição de natureza:
  - venda
  - remessa
  - devolução
  - bonificação

---

### Produtos elegíveis
- filtragem de códigos válidos
- exclusão de produtos fora da regra

---

### Grupo comercial
- agrupamento de clientes via SBM / A1_TELEX
- consolidação de carteira

---

# 6. MARTS

## Objetivo

Estruturar dados para consumo analítico.

## Saídas principais

- tabelas fato
- dimensões

---

## Exemplos

- fct_pedido_item
- fct_faturamento_item
- dim_cliente
- dim_produto
- dim_vendedor
- dim_grupo_comercial

---

# 7. SEMANTIC LAYER

## Objetivo

Definir métricas oficiais da empresa.

---

## Características

- cálculos padronizados
- KPIs oficiais
- governança de métricas

---

## Exemplos

- faturamento líquido
- margem
- carteira
- forecast
- ticket médio

---

# 8. Power BI (Consumo)

## Objetivo

Visualização dos dados.

---

## Características

- somente leitura
- sem regras de negócio
- sem transformações complexas
- consumo de métricas oficiais

---

# Princípio central do fluxo

```text
Quanto mais perto do ERP → mais técnico
Quanto mais perto do BI → mais semântico
```

---

# Resultado final

Esse fluxo garante:

- rastreabilidade total
- consistência de métricas
- separação de responsabilidades
- governança de dados
- escalabilidade da arquitetura