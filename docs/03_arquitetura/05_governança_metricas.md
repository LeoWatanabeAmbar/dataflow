# Governança de Métricas

## Objetivo

Definir as regras de governança para criação, manutenção e uso das métricas corporativas, garantindo consistência entre relatórios, dashboards e análises.

---

# Princípio Central

Toda métrica oficial da empresa deve possuir:

- definição única
- lógica centralizada
- rastreabilidade até a origem
- documentação formal

---

# O que é uma métrica oficial

Uma métrica oficial é qualquer indicador utilizado para tomada de decisão.

Exemplos:
- faturamento
- margem
- carteira
- forecast
- ticket médio
- quantidade vendida

---

# Camada responsável

Todas as métricas oficiais devem ser definidas exclusivamente na:

```text
SEMANTIC LAYER
```

---

# O que NÃO é permitido

## 1. Métrica no Power BI

Não é permitido criar métricas com lógica de negócio diretamente no Power BI.

Exemplo proibido:
- cálculos de triangulação
- filtros de CFOP
- regras de programação comercial

---

## 2. Métrica duplicada

A mesma métrica não pode existir com definições diferentes em relatórios distintos.

---

## 3. Métrica sem documentação

Toda métrica deve possuir definição formal.

---

# Tipos de Métricas

## 1. Métricas financeiras

- faturamento bruto
- faturamento líquido
- impostos
- margem

---

## 2. Métricas comerciais

- carteira
- pedidos
- conversão
- ticket médio

---

## 3. Métricas operacionais

- quantidade
- entrega
- programação
- backlog

---

# Regras de Definição

## 1. Fonte única de verdade

Toda métrica deve ser derivada de:

```text
MARTS + INTERMEDIATE
```

---

## 2. Reutilização obrigatória

Nenhuma métrica deve ser recriada em outro contexto.

---

## 3. Consistência absoluta

O valor da métrica deve ser idêntico em qualquer ferramenta.

---

# Governança de Transformações

## INTERMEDIATE

Responsável por:
- cálculos intermediários
- regras de negócio
- derivação de campos

---

## MARTS

Responsável por:
- estrutura final
- agregações base
- modelagem dimensional

---

## SEMANTIC LAYER

Responsável por:
- definição oficial das métricas
- padronização corporativa
- versionamento conceitual

---

# Exemplos de Métricas Governadas

## Faturamento líquido

Definição única:

```text
Soma do faturamento bruto menos impostos e ajustes definidos na camada INTERMEDIATE
```

---

## Carteira

Definição:

```text
Pedidos classificados como Programado ou Carteira conforme regra de programação comercial
```

---

## Margem

Definição:

```text
Faturamento líquido menos custo associado ao item
```

---

# Problemas que a governança resolve

- múltiplas versões da mesma métrica
- divergência entre relatórios
- perda de confiança nos dados
- regras duplicadas em dashboards
- inconsistência entre áreas

---

# Princípios de Governança

## 1. Centralização

Toda métrica deve existir em um único local.

---

## 2. Transparência

Qualquer usuário deve conseguir entender a origem da métrica.

---

## 3. Rastreabilidade

Toda métrica deve ser rastreável até o ERP.

---

## 4. Versionamento

Mudanças em métricas devem ser controladas e documentadas.

---

# Resultado final

Com essa governança, a empresa garante:

- consistência analítica
- confiança nos dados
- escalabilidade de BI
- redução de retrabalho
- padronização corporativa