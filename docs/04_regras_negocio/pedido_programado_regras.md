# Regra de Negócio — Programação Comercial

## Objetivo

Documentar as regras utilizadas para identificação e classificação de pedidos programados na operação comercial da empresa.

A regra tem como objetivo:
- identificar faturamentos programados
- classificar carteira futura
- apoiar previsões comerciais
- estruturar análises de pipeline
- padronizar critérios de programação comercial

---

# Contexto de Negócio

Determinados pedidos possuem informações operacionais indicando:
- previsão futura de faturamento
- programação comercial
- programação logística
- previsão operacional

Essas informações são registradas operacionalmente no campo:

```text
SC5.C5_ZZMODF2
```

O campo possui natureza textual e poderá conter:
- observações comerciais
- comentários operacionais
- datas previstas
- instruções de faturamento

---

# Identificação da Programação

## Origem da Regra

Campo utilizado:

```text
SC5.C5_ZZMODF2
```

---

# Extração de Datas

A regra realiza:
- remoção de caracteres não numéricos
- identificação de sequências numéricas
- interpretação de possíveis datas

Formato esperado:

```text
DDMMYYYY
```

---

# Critérios de Validação

Uma data será considerada válida quando:
- possuir 8 dígitos
- ano entre 2025 e 2027
- mês válido
- dia válido

Datas inválidas serão desconsideradas.

---

# Seleção da Data Oficial

Quando múltiplas datas forem identificadas:
- será utilizada a maior data encontrada

Critério atual:

```text
Maior data válida encontrada no texto
```

---

# Normalização da Data

A data identificada será convertida para:

```text
Primeiro dia do mês
```

Objetivo:
- padronização analítica
- visão mensal de programação
- simplificação das análises de forecast

---

# Classificação Comercial

## Programado

Um pedido será classificado como:

```text
Programado
```

quando:
- a data programada estiver a partir do próximo mês
- e dentro do limite de até 2 anos futuros

---

## Carteira

Um pedido será classificado como:

```text
Carteira
```

quando:
- não possuir programação válida
- possuir programação fora da janela definida
- possuir data inválida
- não possuir data identificável

---

# Janela de Programação

Critério atual:

| Regra | Valor |
|---|---|
| Data mínima | Primeiro dia do próximo mês |
| Data máxima | Hoje + 2 anos |

---

# Impactos Analíticos

A regra impacta diretamente:
- forecast comercial
- pipeline de faturamento
- carteira futura
- planejamento comercial
- planejamento operacional
- metas
- indicadores gerenciais

---

# Riscos Identificados

- preenchimento manual inconsistente
- ausência de padronização textual
- datas inválidas no campo operacional
- múltiplas datas conflitantes
- alterações manuais posteriores
- dependência de interpretação textual

---

# Observações Importantes

O campo:

```text
C5_ZZMODF2
```

não possui estrutura formal de datas.

A programação comercial depende diretamente:
- da qualidade do preenchimento operacional
- da consistência das observações comerciais
- da manutenção dos padrões utilizados pela operação

A interpretação da programação possui natureza:
- operacional
- comercial
- analítica

e não representa necessariamente:
- obrigação contratual
- faturamento garantido
- compromisso fiscal definitivo

---

# Direcionamento Técnico Futuro

A regra deverá futuramente ser centralizada em:
- modelos dbt
- camada semântica corporativa
- regras oficiais de forecast
- modelos intermediários

Estrutura prevista:

```text
RAW → STAGING → INTERMEDIATE → MARTS
```

Possível implementação futura:

```text
int_programacao_comercial
```

ou

```text
int_forecast_comercial
```

---

# Entidades Impactadas

A regra impacta diretamente:
- fct_pedido_item
- dashboards comerciais
- forecast
- pipeline comercial
- indicadores gerenciais
- análises de carteira