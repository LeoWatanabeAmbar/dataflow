# SA1G10 — Dicionário de Campos

## Objetivo

Documentar os principais campos operacionais da entidade SA1G10, responsável pelo cadastro de clientes no ERP TOTVS.

O objetivo deste documento é centralizar:
- significado dos campos
- comportamento operacional
- observações importantes
- riscos conhecidos
- direcionamentos analíticos futuros

---

# Visão Geral

A entidade SA1G10 representa o cadastro de clientes utilizado nas operações comerciais e fiscais do ERP.

Cada registro representa um cliente/loja.

A identificação do cliente é composta por:

```text
A1_COD + A1_LOJA
```

A entidade é utilizada em:
- pedidos de venda
- faturamento
- financeiro
- análises comerciais
- relatórios operacionais

---

# Campos Operacionais

| Campo | Nome Semântico | Descrição | Observação |
|---|---|---|---|
| A1_COD | Código do Cliente | Identificador principal do cliente | Utilizado junto com A1_LOJA |
| A1_LOJA | Loja do Cliente | Identificador secundário do cliente | Diferencia unidades/filiais do cliente |
| A1_PESSOA | Tipo de Pessoa | Define se o cliente é pessoa física ou jurídica | Normalmente "F" ou "J" |
| A1_CGC | CPF/CNPJ | Documento fiscal do cliente | Pode conter formatação inconsistente |
| A1_NOME | Nome do Cliente | Nome completo/razão social do cliente | Nome operacional do cadastro |
| A1_NREDUZ | Nome Reduzido | Nome reduzido/fantasia do cliente | Utilizado em relatórios e operações |
| A1_TIPO | Tipo de Cliente | Classificação operacional do cliente | Regra depende da operação |
| A1_TELEX | Código do Grupo Comercial | Código utilizado para agrupamento econômico/comercial do cliente | Relacionamento com SBM/SBMG10 |
| A1_GRPTRIB | Grupo Tributário | Grupo tributário vinculado ao cliente | Utilizado em regras fiscais/comerciais |
| D_E_L_E_T_ | Exclusão Lógica | Indicador de exclusão lógica do ERP TOTVS | Registros deletados logicamente devem ser ignorados |

---

# Campos Relacionais

| Campo | Relacionamento | Entidade | Campo Entidade |
|---|---|---|---|
| A1_COD + A1_LOJA | cliente do pedido | SC5G10 | C5_CLIENTE + C5_LOJACLI |
| A1_COD + A1_LOJA | cliente do faturamento | SD2G10 | D2_CLIENTE + D2_LOJA |
| A1_TELEX | grupo comercial | SBMG10 | BM_GRUPO |

---

# Regras Operacionais Identificadas

## Exclusão lógica

```sql
D_E_L_E_T_ <> '*'
```

Registros deletados logicamente não deverão compor a camada analítica oficial.

---

# Regras Semânticas Identificadas

## Chave operacional

A identificação única do cliente deverá considerar:

```text
A1_COD + A1_LOJA
```

O código do cliente isoladamente poderá possuir múltiplas lojas/unidades.

---

## Natureza da entidade

A SA1 representa o cadastro operacional e fiscal de clientes.

A entidade é utilizada em múltiplos processos:
- comercial
- fiscal
- financeiro
- faturamento

---

## Agrupamento Comercial

O campo:

```text
A1_TELEX
```

é utilizado para agrupar clientes em grupos econômicos/comerciais.

Esse relacionamento permite consolidar:
- obras
- filiais
- lojas
- múltiplos CNPJs

em um único grupo comercial.

O relacionamento ocorre através de:

```text
SA1.A1_TELEX = SBMG10.BM_GRUPO
```

Campos derivados do agrupamento:
- BM_DESC → nome do grupo comercial

Exemplos:

| Código | Grupo Comercial |
|---|---|
| C02 | MRV |
| C03 | TENDA |

Esse agrupamento é utilizado principalmente para:
- análises comerciais
- consolidação de faturamento
- carteira de clientes
- indicadores estratégicos
- visão de grupo econômico

---

# Riscos Identificados

- clientes duplicados
- múltiplas lojas para o mesmo cliente
- CPF/CNPJ preenchido incorretamente
- alterações manuais de cadastro
- inconsistência entre razão social e nome reduzido
- clientes inativos ainda presentes na base
- agrupamentos comerciais preenchidos incorretamente
- grupos econômicos sem padronização operacional

---

# Observações Importantes

O mesmo cliente poderá possuir:
- múltiplas lojas
- múltiplos pedidos
- múltiplos faturamentos

O campo:

```text
A1_NREDUZ
```

normalmente é utilizado em relatórios operacionais e dashboards devido ao nome reduzido/fantasia.

Campos fiscais poderão possuir regras específicas definidas pela operação fiscal da empresa.

O agrupamento comercial não representa necessariamente vínculo societário formal entre empresas.

---

# Direcionamento Futuro

Os campos documentados nesta entidade deverão servir futuramente como origem para:
- dimensões de clientes
- dimensões de grupos econômicos
- regras fiscais
- modelos MARTS
- análises comerciais
- análises de faturamento
- regras centralizadas no dbt

Estrutura prevista:

```text
RAW → STAGING → MARTS
```