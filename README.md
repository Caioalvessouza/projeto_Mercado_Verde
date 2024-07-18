# Projeto_Mercado_Verde

## Descrição da Empresa
A Mercado Verde é uma empresa fictícia especializada na venda de produtos orgânicos e sustentáveis. A empresa se dedica a oferecer produtos de alta qualidade que promovem um estilo de vida saudável e sustentável para seus clientes. A Mercado Verde opera tanto no varejo físico quanto no e-commerce, abrangendo uma ampla gama de produtos desde alimentos frescos a itens de higiene pessoal.

## Objetivo do Projeto
O objetivo deste projeto é analisar os dados de vendas, clientes, produtos e fornecedores para obter insights valiosos que possam auxiliar a empresa a tomar decisões estratégicas. Através da criação e consulta de um banco de dados, buscamos responder perguntas-chave sobre o desempenho e comportamento de vendas.

# Principais Problemas e Questões

## Média Mensal de Renda dos Clientes

 - Calcular a média da renda mensal dos clientes para entender o perfil econômico da base de clientes.

## Fornecedores com Maior Quantidade de Produtos

- Identificar quais fornecedores fornecem a maior quantidade de produtos para otimizar o gerenciamento de estoque e parcerias.

## Mês com Maior Volume de Vendas

- Determinar qual mês teve o maior volume de vendas para analisar sazonalidade e planejar campanhas de marketing.

## Faixa Etária de Clientes que Compra Mais Produtos Orgânicos

- Descobrir quais faixas etárias estão mais inclinadas a comprar produtos orgânicos para direcionar estratégias de marketing.

## Ticket Médio das Vendas

- Calcular o ticket médio das vendas para entender o valor médio gasto por transação e ajustar políticas de preços e promoções.

## Categoria de Produtos Mais Vendida

- Identificar a categoria de produtos mais vendida para otimizar o mix de produtos e melhorar a oferta aos clientes.

# Estratégia da Solução

## Etapas nas Consultas em SQL
 ### Criação de Tabelas
 
- Clientes: Contém informações sobre os clientes, incluindo ID, nome, idade, e renda mensal.
- Produtos: Inclui detalhes dos produtos, como ID, nome, categoria, e quantidade disponível.
- Vendas: Registra as transações de vendas, com ID do cliente, ID do produto, quantidade e valor total.
- Fornecedores: Dados sobre fornecedores, incluindo ID e nome.

## Consultas SQL Principais

- Média Mensal de Renda dos Clientes
#### SELECT AVG(Renda_Mensal) as Media_Renda_Mensal FROM Clientes;

![pergunta-1](img/pergunta-1.png)

- Fornecedor com Maior Quantidade de Produtos
  #### SELECT 
  ####  f.ID_Fornecedor,
  ####  f.Nome_Fornecedor,
  ####  SUM(p.Quantidade_Disponível) as Quantidade_Total_Disponível
  #### FROM 
  #### Fornecedores f
  #### JOIN 
  ####  Produtos p ON f.ID_Fornecedor = p.Fornecedor
  #### GROUP BY 
  #### f.ID_Fornecedor, f.Nome_Fornecedor
  #### ORDER BY 
  #### Quantidade_Total_Disponível DESC
  #### LIMIT 1;

----------------------------
- Mês com Maior Volume de Vendas
  #### SELECT 
  ####  strftime('%Y-%m', Data) as Mes,
  ####  SUM(Quantidade) as Quantidade_Total,
  ####  SUM(Valor_Total) as Valor_Total_Vendas
  ####  FROM 
  #### Vendas
  #### GROUP BY 
  #### Mes
  #### ORDER BY 
  #### Valor_Total_Vendas DESC
  #### LIMIT 1;
-------------------------------------
- Faixa Etária de Clientes que Compra Mais Produtos Orgânicos
  #### WITH Faixas_Etarias AS (
  ####  SELECT 
  ####      c.ID_Cliente,
  ####      c.Nome,
  ####      c.Idade,
  ####      v.Quantidade,
  ####      v.Valor_Total,
  ####      CASE
  ####          WHEN c.Idade BETWEEN 0 AND 18 THEN '0-18'
  ####          WHEN c.Idade BETWEEN 19 AND 30 THEN '19-30'
  ####          WHEN c.Idade BETWEEN 31 AND 45 THEN '31-45'
  ####          WHEN c.Idade BETWEEN 46 AND 60 THEN '46-60'
  ####          ELSE '61+'
  ####      END AS Faixa_Etaria
  ####  FROM 
  ####      Clientes c
  ####  JOIN 
  ####      Vendas v ON c.ID_Cliente = v.ID_Cliente
  ####  JOIN
  ####     Produtos p ON v.ID_Produto = p.ID_Produto
  ####  WHERE 
  ####      p.Nome_Produto LIKE '%Orgânico%'
  #### )
  #### SELECT 
  ####  Faixa_Etaria,
  ####  SUM(Quantidade) as Quantidade_Total,
  ####  SUM(Valor_Total) as Valor_Total_Comprado
  #### FROM 
  ####  Faixas_Etarias
  #### GROUP BY 
  ####  Faixa_Etaria
  #### ORDER BY 
  ####  Quantidade_Total DESC;
--------------------------------------------------

- Ticket Médio das Vendas
  #### SELECT 
  ####  SUM(Valor_Total) / COUNT(*) as Ticket_Medio
#### FROM 
####    Vendas;

--------------------------------------------------
- Categoria de Produtos Mais Vendida
#### SELECT 
####    p.Categoria,
####    SUM(v.Quantidade) as Quantidade_Total_Vendida
#### FROM 
####    Vendas v
#### JOIN 
####    Produtos p ON v.ID_Produto = p.ID_Produto
#### GROUP BY 
####  p.Categoria
#### ORDER BY 
####   Quantidade_Total_Vendida DESC
#### LIMIT 1;
--------------------------------------------
## Conclusão
A análise dos dados da Mercado Verde forneceu diversos insights valiosos:

#### Perfil Econômico dos Clientes:

A média da renda mensal dos clientes ajuda a entender melhor o poder de compra e ajustar as estratégias de preço e produtos oferecidos.
#### Gerenciamento de Fornecedores e Estoque:

Identificar os fornecedores com maior quantidade de produtos permite uma melhor gestão de parcerias e estoque.
#### Sazonalidade das Vendas:

Conhecer o mês com maior volume de vendas auxilia no planejamento de campanhas de marketing e promoções sazonais.
#### Segmentação de Mercado:

Saber quais faixas etárias compram mais produtos orgânicos permite uma segmentação de mercado mais precisa e campanhas de marketing direcionadas.
#### Ticket Médio:

Calcular o ticket médio das vendas é essencial para entender o comportamento de compra dos clientes e ajustar as políticas de preços e promoções.
##### Mix de Produtos:

Identificar a categoria de produtos mais vendida ajuda a ajustar o mix de produtos e garantir que as ofertas atendam à demanda do mercado.
Esses insights são fundamentais para que a Mercado Verde possa tomar decisões estratégicas informadas, melhorar a satisfação do cliente e otimizar suas operações.


  



