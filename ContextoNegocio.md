# Contexto Atual do Ambiente de BI

## Fluxo Atual

Atualmente, os dashboards e projetos analíticos são construídos utilizando consultas específicas diretamente no banco de dados do ERP TOTVS.

Cada dashboard possui:
- sua própria query
- seus próprios tratamentos
- suas próprias regras de negócio
- transformações realizadas no Power BI

Grande parte da lógica analítica fica distribuída entre:
- SQL
- Power Query
- DAX
- tratamentos locais dentro de cada projeto

O fluxo atual pode ser resumido da seguinte forma:

```text
ERP TOTVS
    ↓
Query específica
    ↓
Tratamentos no Power BI
    ↓
Dashboard
```

---

# Problemas do Modelo Atual

## Reaplicação de Regras

Atualmente, as regras de negócio precisam ser recriadas para cada novo dashboard ou projeto.

Exemplos:
- regras de faturamento
- devoluções
- CFOP
- clientes ativos
- cálculo de CPV
- classificação de produtos
- indicadores financeiros

Isso gera:
- retrabalho constante
- duplicidade de lógica
- aumento da complexidade
- maior tempo de desenvolvimento

---

## Falta de Padronização

Como cada dashboard possui implementações próprias, existe risco de divergência entre indicadores semelhantes.

Exemplo:
- um dashboard pode considerar determinados CFOPs
- outro dashboard pode utilizar critérios diferentes
- uma regra pode ter sido atualizada em um projeto e esquecida em outro

Isso pode fazer com que:
- os números não batam entre dashboards
- áreas diferentes tenham indicadores diferentes
- exista perda de confiança nos dados

---

## Dependência Excessiva do Power BI

Atualmente, o Power BI está sendo utilizado não apenas como ferramenta de visualização, mas também como:
- camada de transformação
- camada de regra de negócio
- camada de integração
- camada analítica

Isso aumenta:
- complexidade dos arquivos
- dificuldade de manutenção
- tempo de atualização
- dificuldade de escalabilidade

---

## Baixa Reutilização

Hoje, novos dashboards normalmente exigem:
- novas queries
- novos tratamentos
- novas validações
- nova implementação de regras já existentes

Mesmo quando o projeto utiliza dados semelhantes aos dashboards anteriores.

---

## Impacto Operacional

O modelo atual também aumenta:
- dependência do banco transacional
- consumo direto do ERP
- risco de consultas pesadas
- dificuldade de governança

---

# Motivação da Nova Arquitetura

A nova arquitetura surge para centralizar:
- transformação de dados
- regras de negócio
- métricas oficiais
- camada analítica reutilizável

O objetivo é criar uma estrutura onde:
- as regras sejam implementadas apenas uma vez
- os dashboards reutilizem a mesma camada analítica
- os indicadores sejam padronizados
- o Power BI fique responsável principalmente pela visualização

---

# Objetivo da Mudança

A proposta é evoluir para uma arquitetura moderna utilizando:

```text
SQL Server → Supabase → dbt → Power BI
```

Com isso, espera-se:
- eliminar retrabalho
- aumentar reutilização
- melhorar performance
- padronizar indicadores
- reduzir divergências entre dashboards
- facilitar manutenção
- criar uma base escalável para futuras iniciativas de dados e IA