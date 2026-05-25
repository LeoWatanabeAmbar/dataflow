# Modelagem de Dados

## Objetivo

Definir a estrutura de modelagem dimensional utilizada na arquitetura analítica do projeto, garantindo consistência entre regras de negócio e consumo no BI.

---

# Princípios da Modelagem

## 1. Modelo dimensional

A arquitetura segue o modelo estrela (star schema), com:

- fatos (medidas)
- dimensões (contexto)

---

## 2. Separação entre fato e dimensão

- Fatos representam eventos
- Dimensões representam entidades

---

## 3. Granularidade

Cada fato deve ter uma granularidade única e bem definida.

---

# Entidades de Fato

## fct_pedido_item

Representa os itens do pedido de venda.

Origem:
- SC6G10

Granularidade:
- 1 linha por item de pedido

Principais métricas:
- quantidade
- valor
- desconto
- preço unitário

---

## fct_faturamento_item

Representa os itens faturados.

Origem:
- SD2G10

Granularidade:
- 1 linha por item de nota fiscal

Principais métricas:
- valor bruto
- valor líquido
- impostos
- custo

---

# Dimensões

## dim_cliente

Origem:
- SA1G10

Inclui:
- cliente
- loja
- grupo comercial

---

## dim_produto

Origem:
- SB1G10

Inclui:
- código
- descrição
- tipo
- categoria fiscal

---

## dim_vendedor

Origem:
- SA3G10

Inclui:
- representante
- executivo
- vendedor interno

---

## dim_grupo_comercial

Origem:
- SBM + SA1.A1_TELEX

Inclui:
- agrupamento econômico de clientes

---

## dim_cfop

Origem:
- regra de negócio (CFOP mapping)

Inclui:
- tipo de operação
- classificação fiscal

---

## dim_tempo

Origem:
- derivada de datas operacionais

Inclui:
- ano
- mês
- trimestre
- dia

---

# Regras de Modelagem Importantes

## 1. Triangulação

A triangulação impacta diretamente a modelagem de fatos:

- duplicação de registros
- rateio de valores
- múltiplos responsáveis comerciais

---

## 2. Evitar duplicidade de faturamento

Faturamento e pedido são entidades separadas.

Nunca devem ser somados diretamente sem regra de negócio.

---

## 3. Consistência entre camadas

- STAGING não define modelo
- INTERMEDIATE aplica regras
- MARTS estrutura fatos e dimensões

---

# Problemas evitados com essa modelagem

- duplicidade de receita
- inconsistência de margem
- divergência entre BI e ERP
- múltiplas versões de métricas
- lógica espalhada em dashboards

---

# Resultado final

A modelagem garante:

- estrutura clara de fatos e dimensões
- consistência entre áreas
- governança de métricas
- escalabilidade do BI