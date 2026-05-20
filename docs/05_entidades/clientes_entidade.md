# Entidade — dim_cliente

## Objetivo

Consolidar as informações cadastrais, comerciais e fiscais dos clientes utilizados nas operações da empresa.

A entidade tem como objetivo centralizar:
- identificação do cliente
- dados cadastrais
- agrupamento comercial/econômico
- informações fiscais
- relacionamento comercial

A dimensão será utilizada como base para análises:
- comerciais
- faturamento
- carteira de clientes
- grupos econômicos
- indicadores estratégicos

---

# Granularidade

Cada registro representa:

```text
1 cliente + 1 loja
```

Chave operacional:

```text
A1_COD + A1_LOJA
```

---

# Origens

| Entidade | Objetivo |
|---|---|
| SA1G10 | Cadastro principal do cliente |
| SBMG10 | Grupo comercial/econômico |

---

# Relacionamentos Principais

| Entidade Relacionada | Tipo |
|---|---|
| fct_pedido_item | cliente do pedido |
| fct_faturamento_item | cliente do faturamento |
| dim_grupo_comercial | agrupamento econômico/comercial |

---

# Principais Atributos

| Atributo | Origem | Descrição |
|---|---|---|
| codigo_cliente | SA1.A1_COD | Código principal do cliente |
| loja_cliente | SA1.A1_LOJA | Loja/unidade do cliente |
| cliente | SA1.A1_NOME | Nome completo/razão social |
| cliente_reduzido | SA1.A1_NREDUZ | Nome reduzido/fantasia |
| tipo_pessoa | SA1.A1_PESSOA | Física/Jurídica |
| cpf_cnpj | SA1.A1_CGC | Documento fiscal |
| grupo_comercial_codigo | SA1.A1_TELEX | Código do grupo comercial |
| grupo_comercial | SBM.BM_DESC | Nome do grupo comercial |
| grupo_tributario | SA1.A1_GRPTRIB | Grupo tributário |

---

# Regras Operacionais

## Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

Registros deletados logicamente não deverão compor a dimensão oficial.

---

# Regras Semânticas

## Cliente operacional

A dimensão representa o cliente operacional utilizado nas transações comerciais e fiscais.

Um mesmo cliente poderá possuir:
- múltiplas lojas
- múltiplos pedidos
- múltiplos faturamentos

---

## Grupo Comercial

O agrupamento comercial permite consolidar diferentes:
- obras
- filiais
- CNPJs
- unidades comerciais

em um único grupo econômico/comercial.

Relacionamento:

```text
SA1.A1_TELEX = SBMG10.BM_GRUPO
```

Exemplos:
- MRV
- TENDA
- DIRECIONAL

---

# Métricas Relacionadas

A entidade será utilizada para composição de:
- faturamento por cliente
- faturamento por grupo econômico
- margem por cliente
- carteira ativa
- volume vendido
- ticket médio
- concentração de clientes

---

# Riscos Identificados

- clientes duplicados
- múltiplas lojas para o mesmo cliente
- CPF/CNPJ inconsistente
- grupos econômicos preenchidos incorretamente
- alterações manuais no cadastro
- clientes inativos ainda vinculados a operações históricas

---

# Observações Importantes

O agrupamento comercial não representa necessariamente vínculo societário formal.

A qualidade analítica da entidade depende diretamente:
- da manutenção do cadastro de clientes
- da consistência do agrupamento comercial
- da padronização operacional

Campos fiscais poderão possuir regras específicas conforme operação tributária.

---

# Direcionamento Futuro

A entidade deverá futuramente servir como base para:
- camada MARTS
- indicadores comerciais
- segmentação de clientes
- análise de grupos econômicos
- modelos semânticos
- regras centralizadas no dbt

Estrutura prevista:

```text
RAW → STAGING → DIM_CLIENTE
```