# SB1G10 — Dicionário de Campos

## Objetivo

Documentar os principais campos operacionais da entidade SB1G10, responsável pelo cadastro de produtos no ERP TOTVS.

O objetivo deste documento é centralizar:
- significado dos campos
- comportamento operacional
- observações importantes
- riscos conhecidos
- direcionamentos analíticos futuros

---

# Visão Geral

A entidade SB1G10 representa o cadastro de produtos utilizados nas operações comerciais, fiscais, produtivas e logísticas do ERP.

Cada registro representa um produto.

A entidade é utilizada em:
- pedidos de venda
- faturamento
- estoque
- produção
- custos
- análises comerciais
- relatórios operacionais

O filtro operacional atualmente utilizado considera apenas produtos válidos para análise comercial:

```sql
LEN(B1_COD) = 6
AND (
    B1_COD LIKE 'PX%'
    OR B1_COD LIKE 'PI%'
    OR B1_COD LIKE '2M%'
    OR B1_COD LIKE '00%'
)
```

---

# Campos Operacionais

| Campo | Nome Semântico | Descrição | Observação |
|---|---|---|---|
| B1_COD | Código do Produto | Identificador operacional do produto | Chave operacional |
| B1_DESC | Descrição do Produto | Descrição principal do produto | Nome operacional/comercial |
| B1_TIPO | Tipo do Produto | Classificação operacional do produto | Define comportamento operacional/fiscal |
| B1_IPI | Percentual de IPI | Percentual de IPI vinculado ao produto | Utilizado em operações fiscais |
| B1_POSIPI | NCM | Código NCM/posição fiscal do produto | Utilizado em regras fiscais |
| B1_ZZCMO | Custo Médio Operacional | Campo customizado de custo operacional | Utilizado em análises internas |
| D_E_L_E_T_ | Exclusão Lógica | Indicador de exclusão lógica do ERP TOTVS | Registros deletados logicamente devem ser ignorados |

---

# Campos Relacionais

| Campo | Relacionamento | Entidade | Campo Entidade |
|---|---|---|---|
| B1_COD | produto do pedido | SC6G10 | C6_PRODUTO |
| B1_COD | produto faturado | SD2G10 | D2_COD |
| B1_COD | movimentação de estoque | SB2G10 | B2_COD |

---

# Regras Operacionais Identificadas

## Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

Registros deletados logicamente não deverão compor a camada analítica oficial.

---

## Produtos elegíveis para análise

```sql
LEN(B1_COD) = 6
AND (
    B1_COD LIKE 'PX%'
    OR B1_COD LIKE 'PI%'
    OR B1_COD LIKE '2M%'
    OR B1_COD LIKE '00%'
)
```

O modelo analítico atual considera apenas produtos dentro desse padrão operacional.

---

# Regras Semânticas Identificadas

## Natureza da entidade

A SB1 representa o cadastro mestre de produtos.

A entidade centraliza informações:
- comerciais
- fiscais
- operacionais
- produtivas

---

## Classificação operacional dos produtos

Os prefixos dos produtos possuem significado operacional/comercial.

Exemplos atuais:
- PX
- PI
- 2M
- 00

A interpretação definitiva dos prefixos deverá ser validada com as regras de negócio da operação.

---

# Riscos Identificados

- produtos inativos ainda utilizados historicamente
- classificações operacionais inconsistentes
- descrições alteradas manualmente
- produtos cadastrados fora do padrão
- utilização incorreta de tipos de produto
- divergência entre custo operacional e custo contábil
- inconsistência fiscal de NCM/IPI

---

# Observações Importantes

O cadastro de produtos é utilizado por múltiplos módulos do ERP.

Alterações no cadastro poderão impactar:
- análises comerciais
- fiscal
- estoque
- custos
- produção

O campo:

```text
B1_ZZCMO
```

possui natureza customizada e representa um custo operacional utilizado internamente pela operação.

O custo operacional poderá divergir:
- do custo médio contábil
- do custo fiscal
- do custo de reposição

---

# Direcionamento Futuro

Os campos documentados nesta entidade deverão servir futuramente como origem para:
- dimensões de produtos
- classificações comerciais
- regras fiscais
- análises de custo
- modelos MARTS
- regras centralizadas no dbt

Estrutura prevista:

```text
RAW → STAGING → MARTS
```