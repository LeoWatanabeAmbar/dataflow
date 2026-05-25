# CFOP — Classificação de Natureza da Operação

## Objetivo

Definir uma regra única de classificação dos CFOPs utilizados nos itens de venda e faturamento, garantindo padronização analítica entre pedido (SC6) e faturamento (SD2).

---

# Fonte dos Dados

Campo base:

```text
C6_CF (SC6G10)
D2_CF (SD2G10)
```

---

# Problema que esta regra resolve

O CFOP no ERP é:
- técnico
- operacional
- inconsistente para análise direta
- não padronizado para BI

---

# Camada responsável

Esta regra pertence à camada:

```text
INTERMEDIATE
```

---

# Regra de Classificação

Os CFOPs são agrupados em categorias analíticas.

---

## 1. VENDA

Operações de venda para cliente final.

```text
5101, 5102, 5116, 5120, 5122, 5123, 5124, 5125,
5401, 5403, 5405, 5933,
6101, 6102, 6107, 6108, 6109, 6116, 6120, 6122, 6123, 6124, 6125,
6401, 6403, 6501, 6502, 6933,
7101, 7102
```

---

## 2. REMESSA

Operações de movimentação sem venda direta.

Exemplos típicos:
- remessa para industrialização
- remessa para demonstração
- transferência física sem faturamento

```text
5901, 5902, 5904, 5905, 5906, 5907, 5908, 5910
6901, 6902, 6904, 6905, 6906, 6907, 6908, 6910
```

---

## 3. DEVOLUÇÃO

Operações de retorno de mercadoria.

```text
1201, 1202, 2201, 2202, 5201, 5202, 6201, 6202
```

---

## 4. TRANSFERÊNCIA

Movimentação entre filiais ou unidades.

```text
5151, 5152, 5153, 5154,
5551, 5552,
6151, 6152, 6153, 6154,
6551, 6552
```

---

## 5. BONIFICAÇÃO

Operações sem valor comercial.

```text
5910, 6910, 1910
```

---

# Regra de Implementação

## Classificação final

```sql
CASE
    WHEN CFOP IN (lista_venda) THEN 'VENDA'
    WHEN CFOP IN (lista_remessa) THEN 'REMESSA'
    WHEN CFOP IN (lista_devolucao) THEN 'DEVOLUCAO'
    WHEN CFOP IN (lista_transferencia) THEN 'TRANSFERENCIA'
    WHEN CFOP IN (lista_bonificacao) THEN 'BONIFICACAO'
    ELSE 'OUTROS'
END
```

---

# Regras Importantes

## 1. Prioridade do CFOP

A classificação deve ser baseada no CFOP do **faturamento (SD2)** sempre que disponível.

Se não existir:
→ usar CFOP do pedido (SC6)

---

## 2. Não confiar no ERP como verdade analítica

O CFOP:
- pode estar inconsistente no pedido
- pode mudar no faturamento
- pode ser influenciado por operação interna

---

## 3. Regra de consistência

Pedido e faturamento podem ter CFOP diferentes.

A análise oficial sempre deve priorizar:

```text
SD2 (faturamento)
```

---

# Uso na Arquitetura

Essa regra alimenta:

- fct_faturamento_item
- fct_pedido_item
- cálculo de receita
- margem
- triangulação
- programação comercial

---

# Impacto nas Métricas

## VENDA
- entra em faturamento
- compõe receita

## REMESSA
- não deve compor receita
- tratado separadamente

## DEVOLUÇÃO
- reduz faturamento

## TRANSFERÊNCIA
- neutro analiticamente

## BONIFICAÇÃO
- sem valor financeiro

---

# Resultado esperado

Após essa regra:

- CFOP deixa de ser campo técnico
- vira dimensão analítica padronizada
- garante consistência entre todos os relatórios