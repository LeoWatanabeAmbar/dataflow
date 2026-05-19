# Contexto dos Dados

## Origem dos Dados

Atualmente, os dados utilizados no ambiente analítico da empresa possuem como principal origem o ERP TOTVS Protheus, armazenado em SQL Server.

Os dashboards e análises acessam diretamente tabelas transacionais do ERP através de consultas SQL específicas para cada necessidade de negócio.

Além do ERP, existem também tratamentos complementares realizados localmente em:
- Power Query
- DAX
- scripts Python
- arquivos auxiliares

---

# Características do Ambiente Atual

O ambiente atual é altamente acoplado ao sistema transacional, onde:
- dashboards consultam diretamente o ERP
- regras de negócio são implementadas em múltiplos locais
- transformações ocorrem dentro dos projetos analíticos
- não existe uma camada analítica centralizada

Grande parte da lógica de dados encontra-se distribuída entre:
- consultas SQL
- modelos Power BI
- scripts auxiliares
- tratamentos específicos de cada dashboard

---

# Estrutura Atual dos Dados

Atualmente, os dados são consumidos diretamente de tabelas operacionais do TOTVS, sem separação clara entre:
- dados transacionais
- dados analíticos
- regras de negócio
- métricas oficiais

Isso faz com que os dashboards dependam diretamente:
- da estrutura do ERP
- da performance do banco transacional
- da lógica implementada individualmente em cada projeto

Além disso, o ambiente atual não possui uma camada semântica oficial para padronização dos indicadores utilizados pelas áreas de negócio.

---

# Regras Gerais do ERP

## Exclusão lógica

O ERP TOTVS utiliza exclusão lógica de registros através do campo:

```text
D_E_L_E_T_
```

Todos os modelos analíticos futuros deverão considerar tratamento adequado para exclusão de registros deletados logicamente.

---

# Regras Semânticas Iniciais

## Confirmação de faturamento

A existência de data de faturamento em pedidos de venda não garante faturamento efetivo.

Um item somente deverá ser considerado faturado quando existir documento fiscal associado à operação.

A confirmação do faturamento deverá ocorrer prioritariamente através das entidades fiscais e itens faturados.

---

## Classificação fiscal das operações

A natureza da operação será definida através do CFOP.

O CFOP será utilizado para classificar operações como:
- venda
- remessa
- devolução
- transferência
- outras classificações futuras

Inicialmente, parte dos CFOPs será classificada como:

```text
VENDA
```

e os demais como:

```text
REMESSA
```

Essa regra deverá futuramente ser centralizada em estrutura própria de classificação fiscal, evitando replicação de lógica em dashboards e modelos analíticos.

---

## Elegibilidade dos itens comerciais

Somente deverão ser considerados válidos analiticamente os itens que não estiverem bloqueados operacionalmente.

Regras iniciais identificadas:

```sql
C6_BLQ <> 'R'
D2_BLQ <> 'R'
```

Itens bloqueados não deverão compor:
- faturamento válido
- carteira comercial
- backlog
- indicadores comerciais

A aplicação dessas regras deverá ser centralizada na camada analítica futura.

---

# Principais Entidades

As principais entidades identificadas inicialmente para o domínio comercial são:

- pedidos de venda
- itens de pedido
- faturamento
- clientes
- produtos
- vendedores

---

# SD2G10 — Itens de Faturamento

Classificação analítica:
Fato transacional fiscal de faturamento.

Granularidade:
Cada linha representa um item faturado de uma nota fiscal, associado a um produto, pedido de venda e documento fiscal.

Responsabilidade:
Representa os itens fiscais faturados das operações comerciais realizadas pela empresa, sendo a principal origem da futura fato de vendas.

Principais informações:
- produto
- quantidade
- número do pedido de venda
- valor
- impostos
- CFOP
- UF de faturamento
- data de faturamento

Relacionamentos:
- SB1G10 → produtos
- SC5G10 → pedidos de venda

Chaves importantes:
- D2_COD → código do produto
- D2_PEDIDO → número do pedido de venda

Uso analítico futuro:
- faturamento bruto
- faturamento líquido
- análise fiscal
- análise comercial
- curva de vendas
- análise geográfica
- análise de mix de produtos

Principais regras futuras:
- exclusão de cancelamentos
- exclusão de registros deletados
- exclusão de itens bloqueados
- definição de faturamento líquido e bruto
- classificação fiscal
- definição dos impostos
- tratamento de devoluções

Observações:
O relacionamento entre pedidos e faturamento não é necessariamente 1:1, podendo existir:
- faturamento parcial
- múltiplos faturamentos para o mesmo pedido
- pedidos não faturados

Nem todo item faturado representa necessariamente receita comercial, pois a classificação da operação dependerá do CFOP da transação.

Itens com:

```text
D2_BLQ = 'R'
```

não deverão ser considerados válidos para composição dos indicadores analíticos oficiais.

A entidade será utilizada futuramente como principal origem da:

```text
marts.fato_vendas
```

---

# SC6G10 — Itens dos Pedidos de Venda

Classificação analítica:
Fato operacional de itens de pedido.

Granularidade:
Cada linha representa um item de um pedido de venda.

Responsabilidade:
Representa os itens comerciais dos pedidos de venda, independentemente do faturamento realizado.

Principais informações:
- produto
- quantidade
- número do pedido
- valor
- impostos
- CFOP
- número da nota fiscal
- data de faturamento

Relacionamentos:
- SB1G10 → produtos
- SC5G10 → pedidos de venda

Chaves importantes:
- C6_PRODUTO → código do produto
- C6_NUM → número do pedido de venda

Uso analítico futuro:
- carteira comercial
- pedidos em aberto
- backlog
- conversão pedido → faturamento
- análise de pedidos
- acompanhamento operacional

Principais regras futuras:
- exclusão de cancelamentos
- exclusão de registros deletados
- exclusão de itens bloqueados
- definição de valor líquido e bruto
- classificação fiscal
- definição dos impostos
- tratamento de pedidos bloqueados

Observações:
A entidade representa intenção/comercialização de venda e não necessariamente faturamento realizado.

Um item somente deverá ser considerado faturado quando possuir documento fiscal associado.

A existência de data de faturamento no pedido não garante faturamento efetivo, indicando apenas vínculo operacional de faturamento.

Itens com:

```text
C6_BLQ = 'R'
```

não deverão ser considerados válidos para composição dos indicadores analíticos oficiais.

Nem todo item de pedido representa faturamento realizado.

A entidade será utilizada futuramente como principal origem da:

```text
marts.fato_pedidos
```

---

# SC5G10 — Pedidos de Venda

Classificação analítica:
Fato operacional de pedidos comerciais.

Granularidade:
Cada linha representa o cabeçalho de um pedido de venda.

Responsabilidade:
Representa os dados gerais e comerciais dos pedidos de venda gerados no ERP.

Principais informações:
- número do pedido
- tipo de frete
- valor do frete
- cliente
- loja do cliente
- município
- UF
- representante comercial
- executivo comercial
- modo de faturamento
- tipo de pedido
- transportadora
- data de emissão

Relacionamentos:
- SA1G10 → clientes
- SA3G10 → vendedores
- SC6G10 → itens dos pedidos
- SD2G10 → itens faturados

Chaves importantes:
- C5_NUM → número do pedido
- cliente → C5_CLIENTE + C5_LOJACLI

Uso analítico futuro:
- análise de carteira
- pedidos em aberto
- backlog comercial
- conversão pedido → faturamento
- lead time comercial
- acompanhamento de faturamento futuro

Principais regras futuras:
- exclusão de cancelamentos
- exclusão de registros deletados
- tratamento de pedidos não faturados
- tratamento de pedidos bloqueados
- validação de pedidos efetivamente faturados

Observações:
Nem todo pedido de venda representa faturamento realizado, sendo necessário tratamento futuro para separação entre:
- carteira
- faturamento
- pedidos cancelados

O campo de modo de faturamento possui natureza operacional e poderá exigir padronização futura devido à possibilidade de preenchimento inconsistente.

---

# SA1G10 — Clientes

Classificação analítica:
Dimensão de clientes.

Granularidade:
Cada linha representa um cadastro de cliente e loja.

Responsabilidade:
Representa os dados cadastrais e comerciais dos clientes utilizados nas operações de venda e faturamento.

Principais informações:
- código do cliente
- loja do cliente
- nome
- nome reduzido
- tipo de cliente
- e-mail
- telefone
- município
- UF
- CNPJ/CPF
- código do grupo de cliente

Relacionamentos:
- SC5G10 → pedidos de venda
- SBMG10 → grupo de clientes

Chaves importantes:
- A1_COD + A1_LOJA → identificação do cliente
- A1_TELEX → código do grupo de cliente

Uso analítico futuro:
- análise de carteira de clientes
- segmentação comercial
- análise geográfica
- análise de recorrência
- curva ABC
- indicadores comerciais

Principais regras futuras:
- exclusão de registros deletados
- padronização cadastral
- consolidação de grupos econômicos

Observações:
O cliente é identificado pela combinação:

```text
A1_COD + A1_LOJA
```

O campo:

```text
A1_TELEX
```

é utilizado operacionalmente para representar o código do grupo de cliente, permitindo agrupamento de múltiplos cadastros em uma entidade comercial consolidada.

Esse relacionamento ocorre com a tabela:

```text
SBMG10
```

responsável pela descrição dos grupos de clientes.

A entidade será utilizada futuramente como principal origem da:

```text
marts.dim_cliente
```

---

# SB1G10 — Produtos

Classificação analítica:
Dimensão de produtos.

Granularidade:
Cada linha representa um cadastro de produto.

Responsabilidade:
Representa os dados descritivos e classificatórios dos produtos utilizados nas operações comerciais e fiscais.

Principais informações:
- código do produto
- descrição do produto
- tipo de produto
- alíquota de IPI

Relacionamentos:
- SC6G10 → itens de pedido
- SD2G10 → itens faturados

Chaves importantes:
- B1_COD → código do produto

Uso analítico futuro:
- análise de mix de produtos
- curva ABC
- análise de faturamento por produto
- segmentação comercial
- análise de linhas e categorias
- indicadores comerciais

Principais regras futuras:
- exclusão de registros deletados
- classificação comercial dos produtos
- padronização de categorias
- consolidação de produtos equivalentes

Observações:
Um mesmo produto pode apresentar mais de um código devido à possibilidade de:
- fabricação própria
- revenda

Produtos revendidos atualmente costumam ser identificados pela sigla:

```text
RV
```

na descrição do produto.

Essa identificação possui natureza operacional e poderá futuramente ser substituída por classificação estruturada e padronizada.

A entidade será utilizada futuramente como principal origem da:

```text
marts.dim_produto
```

---

# Direcionamento Futuro da Modelagem

As entidades atuais do ERP servirão como origem para construção da futura camada dimensional da plataforma analítica.

Estrutura inicial prevista:

## Fatos

```text
marts.fato_vendas
marts.fato_pedidos
```

## Dimensões

```text
marts.dim_cliente
marts.dim_produto
marts.dim_vendedor
marts.dim_tempo
marts.dim_cfop
```

---

# Objetivo da Nova Estrutura de Dados

A nova arquitetura busca criar uma camada analítica desacoplada do ERP, organizada em múltiplas camadas com responsabilidades bem definidas.

O objetivo é separar:
- ingestão de dados
- armazenamento
- transformação
- regras de negócio
- consumo analítico

---

# Estrutura Proposta

```text
SQL Server (ERP)
        ↓
RAW
        ↓
STAGING
        ↓
MARTS
        ↓
Power BI
```

---

# Objetivo Final da Camada de Dados

Ao final da implementação, a plataforma deverá fornecer:
- dados padronizados
- métricas oficiais
- reutilização entre dashboards
- maior governança
- maior rastreabilidade
- maior escalabilidade
- base estruturada para automações e IA
