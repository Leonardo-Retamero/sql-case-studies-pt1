# SQL Cases - Parte 1

Este repositório reúne exercícios resolvidos (cases) do curso de SQL para Análise de Dados oferecido pela Fundação Getulio Vargas (FGV). O objetivo é aplicar na prática os conceitos aprendidos ao longo do curso, com foco em consultas, junções, agregações e filtragens de dados.

Cada case apresenta:

Um enunciado prático

O código SQL utilizado na resolução

Uma breve análise do resultado

---

# 📌 Case 1 — Países Asiáticos com Menor Escolaridade Masculina em 2009

Identificar os cinco países da Ásia em que os homens com 25 anos de idade tiveram, em média, o menor número de anos de estudo no ano de 2009.

Código SQL:

```sql
SELECT m.country, 
       m.mean_years
FROM men_years_at_school m
JOIN country c ON m.country = c.country
WHERE m.ref_year = 2009
  AND c.four_regions LIKE '%asia%'
ORDER BY m.mean_years
LIMIT 5;
```

Análise:

A consulta filtra os países asiáticos no ano de 2009, considerando apenas a média de anos de estudo dos homens com 25 anos. Em seguida, os resultados são ordenados em ordem crescente e são exibidos os cinco países com os menores índices de escolaridade nesse grupo.

Resultado:

![Resultado do Case 1](https://github.com/user-attachments/assets/c2db2904-d3af-4a87-9dbb-34e8d131c2bb)

---

# 📌 Case 2 — Dez Países com Maior Renda Per Capita Diária em 1985

Listar os 10 países com maior renda per capita diária no ano de 1985, indicando também a região e a classificação de renda segundo o Banco Mundial.

Código SQL:

```sql
SELECT ai.country, 
       c.wb_regions,
       c.wb3income,
       ai.mean_usd
FROM avg_income ai 
JOIN country c ON ai.country = c.country
WHERE ai.ref_year = 1985
ORDER BY ai.mean_usd DESC
LIMIT 10;
```

Análise:

A consulta relaciona as tabelas de renda média (avg_income) e de classificação/região do Banco Mundial (country). Ela filtra os dados para o ano de 1985 e ordena os países pela renda média diária em dólares americanos, exibindo os 10 maiores valores. Além da renda, a consulta apresenta a região e a classificação de renda do Banco Mundial para cada país listado.

Resultado:

![Resultado do Case 2](https://github.com/user-attachments/assets/a5821667-7a91-4021-8809-8b73d94c65ae)

---

# 📌 Case 3 — Indicadores Socioeconômicos do Brasil de 1900 a 2020 (a cada 10 anos)

Selecionar os dados do Brasil sobre renda per capita diária, PIB, população, mortalidade infantil, fertilidade e expectativa de vida, para o período de 1900 a 2020, considerando apenas os anos de década (1900, 1910, 1920, ..., 2020).

Código SQL: 

```sql
SELECT ai.ref_year, 
       ai.mean_usd, 
       p.tot_pop, 
       cm.tot_deaths, 
       f.mean_babies, 
       le.tot_years
FROM avg_income ai
JOIN gdp_pc gp ON ai.country = gp.country 
    AND ai.ref_year = gp.ref_year
JOIN population p ON ai.country = p.country 
    AND ai.ref_year = p.ref_year
JOIN child_mortality cm ON ai.country = cm.country 
    AND ai.ref_year = cm.ref_year
JOIN fertility f ON ai.country = f.country 
    AND ai.ref_year = f.ref_year
JOIN life_expectancy le ON ai.country = le.country 
    AND ai.ref_year = le.ref_year
WHERE ai.country = 'Brazil'
  AND ai.ref_year BETWEEN 1900 AND 2020
  AND ai.ref_year % 10 = 0
ORDER BY ai.ref_year;
```

Análise:

A consulta reúne diversos indicadores socioeconômicos para o Brasil, cruzando dados de renda, PIB, população, mortalidade infantil, fertilidade e expectativa de vida. Os dados são filtrados para os anos a cada década, entre 1900 e 2020, permitindo analisar tendências ao longo do tempo.

Resultado:

![Resultado do Case 3](https://github.com/user-attachments/assets/3586455a-7e1a-4863-bd89-602e35def269)

---

# 📌 Case 4 — Mortalidade Infantil e Natalidade nos Menores Países da Europa (Ano 2000)

Pesquisar a taxa de mortalidade infantil e de natalidade nos seis menores países da Europa em termos de extensão territorial — Andorra, Liechtenstein, Malta, Mônaco, San Marino e Vaticano (Holy See) — no ano de 2000. A consulta deve utilizar as tabelas de mortalidade infantil e fertilidade, e aplicar a cláusula `LEFT JOIN` para garantir que todos os países sejam retornados, mesmo que não possuam dados em ambas as tabelas.

Código SQL (Versão 1):

```sql
SELECT cm.country,
       cm.tot_deaths,
       f.mean_babies
FROM child_mortality cm 
LEFT JOIN fertility f ON cm.country = f.country
    AND cm.ref_year = f.ref_year
WHERE cm.country IN ('Andorra', 'Liechtenstein', 'Malta', 'Monaco', 'San Marino', 'Holy See')
  AND cm.ref_year = 2000;
```

Análise (versão 1):

Neste caso, apenas Malta possui dados na tabela de fertilidade para o ano de 2000. Se tivesse sido utilizado um INNER JOIN, somente Malta apareceria no resultado. O uso de LEFT JOIN permite visualizar todos os países, mesmo aqueles sem dados de natalidade. Um ponto curioso é a alta taxa de mortalidade infantil registrada no Vaticano.

![Resultado](https://github.com/user-attachments/assets/58461621-6bfd-478c-9f6a-3ed40ec7468c)

Código SQL (Versão 2):

```sql
SELECT cm.country,
       cm.tot_deaths,
       p.tot_pop, 
       f.mean_babies
FROM child_mortality cm 
LEFT JOIN fertility f ON cm.country = f.country
    AND cm.ref_year = f.ref_year
LEFT JOIN population p ON cm.country = p.country
    AND cm.ref_year = p.ref_year
WHERE cm.country IN ('Andorra', 'Liechtenstein', 'Malta', 'Monaco', 'San Marino', 'Holy See')
  AND cm.ref_year = 2000;
```

Análise (versão 2):

Ao incluir os dados de população, observa-se que o número extremamente reduzido de habitantes no Vaticano pode influenciar artificialmente a taxa de mortalidade infantil, resultando em uma elevação incomum. Essa distorção sugere uma possível inconsistência ou erro nos dados para esse país.

![Resultado](https://github.com/user-attachments/assets/403ccebd-cbe3-4653-9f70-9bfde5b04131)













