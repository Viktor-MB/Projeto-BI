# Business Intelligence — Análise de Vendas ByteShop

Projeto acadêmico de Business Intelligence desenvolvido na IBMEC RJ (orientação: Prof. Anderson Nascimento), cobrindo o ciclo completo de BI: desde a modelagem do Data Warehouse até a entrega de dashboards interativos no Power BI.

---

## O problema

A ByteShop é uma varejista de produtos de informática que tomava decisões estratégicas — gestão de estoque, precificação, campanhas de marketing — com base em relatórios isolados e experiência da equipe. O objetivo do projeto foi construir uma infraestrutura de BI que centralizasse os dados e permitisse análises consistentes sobre vendas, produtos, clientes e funcionários.

---

## Arquitetura da solução

```
PostgreSQL (transacional) → Pentaho PDI (ETL) → Stage Area → Data Warehouse (Star Schema) → Power BI
```

---

## O que foi feito

**Modelagem dimensional**

Star Schema construído no SQL Power Architect com uma tabela fato central e 4 dimensões:

- `ft_vendas` — fato central com métricas de valor total e quantidade
- `dim_produto` — nome, categoria, fabricante, preço
- `dim_cliente` — nome, cidade, estado
- `dim_funcionario` — nome, perfil, sexo
- `dim_data` — dia, mês, ano, dia da semana

Nomenclatura seguindo design patterns: prefixos `ft_`, `dim_`, `sk_`, `nk_`, `md_`.

**Pipeline de ETL no Pentaho**

5 transformações orquestradas por um Job:

- ETL 01: Extração dos dados brutos para a Stage Area (com Truncate para garantir dados frescos a cada carga)
- ETL 02: Carga da `dim_cliente` com SCD Tipo 1 (nome) e SCD Tipo 2 (cidade/estado)
- ETL 03: Carga da `dim_funcionario` com tradução de valores (sexo e perfil) e SCD híbrido
- ETL 04: Carga da `dim_produto` com SCD Tipo 2 para preservar histórico de preço
- ETL 05: Carga da tabela fato `ft_vendas`

**Dashboard no Power BI**

4 painéis analíticos conectados diretamente ao DW em PostgreSQL:

- **Visão Geral:** KPIs de faturamento total, ticket médio, nº de vendas e clientes; sazonalidade e produtos mais vendidos
- **Funcionários:** Faturamento e volume de vendas por vendedor, produtos mais vendidos por sexo, ranking individual
- **Clientes:** Análise geográfica por estado/cidade, correlação entre faturamento e frequência de compra, ranking de clientes
- **Produtos:** Faturamento por categoria (Hardware, Software, Computadores), série temporal por categoria, ticket médio por produto

---

## Tecnologias

- PostgreSQL
- Pentaho Data Integration (PDI)
- SQL Power Architect
- Microsoft Power BI / Fabric
- SQL

---

## Contexto

Projeto em grupo desenvolvido no 3º período do curso de Ciência de Dados e IA — IBMEC RJ. A base transacional simula o ambiente de vendas de uma loja de informática com tabelas de produto, cliente, funcionário e venda.