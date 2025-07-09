# SQL Cases - Parte 1

Este repositório reúne exercícios resolvidos (cases) do curso de SQL para Análise de Dados oferecido pela Fundação Getulio Vargas (FGV). O objetivo é aplicar na prática os conceitos aprendidos ao longo do curso, com foco em consultas, junções, agregações e filtragens de dados.

Cada case apresenta:

Um enunciado prático

O código SQL utilizado na resolução

Uma breve análise do resultado

# 📌 Case 1 — Países Asiáticos com Menor Escolaridade Masculina em 2009

Identificar os cinco países da Ásia em que os homens com 25 anos de idade tiveram, em média, o menor número de anos de estudo no ano de 2009.

Código SQL:

SELECT m.country, 
       m.mean_years
FROM men_years_at_school m
JOIN country c ON m.country = c.country
WHERE m.ref_year = 2009
  AND c.four_regions LIKE '%asia%'
ORDER BY m.mean_years
LIMIT 5;

Análise:
A consulta filtra os países asiáticos no ano de 2009, considerando apenas a média de anos de estudo dos homens com 25 anos. Em seguida, os resultados são ordenados em ordem crescente e são exibidos os cinco países com os menores índices de escolaridade nesse grupo.

Resultado:

![Resultado do Case 1](https://github.com/user-attachments/assets/c2db2904-d3af-4a87-9dbb-34e8d131c2bb)

