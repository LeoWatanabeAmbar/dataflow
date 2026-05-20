# Entidade — dim_produto

## Objetivo

Consolidar as informações cadastrais, comerciais, fiscais e operacionais dos produtos utilizados nas operações da empresa.

A entidade tem como objetivo centralizar:
- identificação dos produtos
- classificações operacionais
- informações fiscais
- custos operacionais
- atributos comerciais

A dimensão será utilizada como base para análises:
- comerciais
- faturamento
- margem
- custos
- mix de produtos
- indicadores operacionais

---

# Granularidade

Cada registro representa:

```text
1 produto
```

Chave operacional:

```text
B1_COD
```

---

# Origens

| Entidade | Objetivo |
|---|---|
| SB1G10 | Cadastro principal de produtos |

---

# Relacionamentos Principais

| Entidade Relacionada | Tipo |
|---|---|
| fct_pedido_item | produto do pedido |
| fct_faturamento_item | produto faturado |
| estoque | movimentação de estoque |

---

# Principais Atributos

| Atributo | Origem | Descrição |
|---|---|---|
| codigo_produto | SB1.B1_COD | Código operacional do produto |
| produto | SB1.B1_DESC | Descrição principal do produto |
| tipo_produto | SB1.B1_TIPO | Classificação operacional do produto |
| percentual_ipi | SB1.B1_IPI | Percentual de IPI |
| ncm | SB1.B1_POSIPI | Código NCM do produto |
| custo_medio_operacional | SB1.B1_ZZCMO | Custo operacional utilizado internamente |

---

# Regras Operacionais

## Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

Registros deletados logicamente não deverão compor a dimensão oficial.

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

A camada analítica considera apenas produtos dentro desse padrão operacional.

---

# Regras Semânticas

## Natureza da entidade

A dimensão representa os produtos comercializados/utilizados nas operações da empresa.

A entidade concentra informações:
- comerciais
- fiscais
- operacionais
- custos

---

## Classificação operacional

Os prefixos dos produtos possuem significado operacional/comercial.

Prefixos atualmente utilizados:
- PX
- PI
- 2M
- 00

A interpretação dos prefixos deverá seguir as regras operacionais da empresa.

---

## Custos Operacionais

O campo:

```text
B1_ZZCMO
```

representa um custo operacional utilizado internamente pela operação.

Esse custo poderá divergir:
- do custo contábil
- do custo fiscal
- do custo médio do estoque

---

# Métricas Relacionadas

A entidade será utilizada para composição de:
- faturamento por produto
- margem
- volume vendido
- mix de produtos
- curva ABC
- custo médio
- rentabilidade
- análises fiscais

---

# Riscos Identificados

- produtos cadastrados fora do padrão
- descrições alteradas manualmente
- classificação operacional inconsistente
- divergência entre custo operacional e contábil
- inconsistência fiscal de NCM/IPI
- produtos inativos ainda presentes historicamente

---

# Observações Importantes

O cadastro de produtos é utilizado por múltiplos módulos do ERP:
- comercial
- fiscal
- estoque
- produção
- custos

Alterações cadastrais poderão impactar diretamente:
- indicadores
- margem
- análises fiscais
- análises comerciais

O custo operacional não deverá ser utilizado isoladamente como custo contábil oficial.

---

# Direcionamento Futuro

A entidade deverá futuramente servir como base para:
- camada MARTS
- análises comerciais
- análises de margem
- indicadores operacionais
- regras fiscais centralizadas
- modelos semânticos
- regras centralizadas no dbt

Estrutura prevista:

```text
RAW → STAGING → DIM_PRODUTO
```