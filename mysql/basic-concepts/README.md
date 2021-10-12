# Comandos Básicos Com MySQL

## Pre-requisitos
Conhecimentos necessários:
* SQL
* Diagrama de Venn (opcional)
* SGBD

<br>

## Instalando o Banco de Dados

[Download MySQL](https://dev.mysql.com/downloads/mysql/)

Durante instalação selecionar:

- [x] Server
- [x] Workbench (opcional)

<br>

## Coments

Fazer comentários:
```sql
--it is comment
```

## Create Database

Criar novo banco de dados:
```sql
CREATE DATABASE example_db;
```

<br>

## Drop Database

Excluir um banco de dados:
```sql
DROP DATABASE example_db;
```

<br>

## Use Database

Selecionar um banco de dados:
```sql
USE DATABASE example_db;
```

<br>

## Create Table

Criar uma tabela:
```sql
CREATE TABLE customers_tb (
    id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    PRYMARY KEY (id);
);
```

<br>

## Alter Table

Altera uma tabela:
```sql
--add columns
ALTER TABLE customers_tb
ADD mobile VARCHAR(20),
ADD example_cl VARCHAR (50);
```

<br>

## Drop Table

Apagar um tabela:
```sql
DROP TABLE example_tb;
```

<br>

## Foreign Key and Table Relationships

Chave estrangeira e relacionamento entre tabelas:
```sql
CREATE TABLE sales_tb (
    id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    price FLOAT NOT NULL,
    customer_id INT NOT NULL,
    PRIMARY KEY (id),
    FOREIGN KEY (customer_id) REFERENCES customers_tb(id)
);
```

<br>

## Insert Into

Inserir dados em uma tabela:
```sql
INSERT INTO sales_tb (name, price, customer_id)
VALUES ('2kg of oranges', 2.99, 1),
       ('1kg of apples', 4.99, 1),
       ('2kg of bananas' 2.99, 2),
       ('2kg of oranges', 2.99, 3);

INSERT INTO customers_tb (name)
VALUES ('Customer_1'),
       ('Customer_2'),
       ('Customer_3');
```

<br>

## Select

Selecionar tudo da tabela *sales_tb*:
```sql
SELECT * FROM sales_tb;
```

<br>

## Specific Column and Limit

Especificar uma coluna e limitar o número de linhas:
```sql
SELECT name FROM sales_tb LIMIT 2;
```

<br>

## As, Alias Columns

Renomear uma coluna ou tabela temporariamente:
```sql
SELECT name, price FROM sales_tb AS a;
```

<br>

## Order By

Ordenar a coluna:
```sql
SELECT * FROM sales_tb ORDER BY name;
```

<br>

## Distinct Select

Não retornar dados repetidos:
```sql
SELECT DISTINCT name FROM sales_tb;
```

<br>

## Update and Where

Atualizar um campo seguindo uma condição:
```sql
UPDATE sales_tb AS a
SET a.price = 3.49
WHERE a.id = 1;
```

<br>

## Comparators

Retornar tuplas onde a condição é verdadeira:
```sql
SELECT * FROM sales_tb AS a
WHERE a.price > 4.0;
```

<br>

## Like String Filter

Retornar se a string conter os caracteres:
```sql
SELECT * FROM sales_tb AS a
WHERE a.name LIKE '%an%';
--may contain characters to the left -> %an
--an% <- may contain characters to the right
```

<br>

## Or and And

Retornar tuplas onde a condição for verdadeira:
```sql
SELECT * FROM sales_tb AS a
WHERE a.price = 3.99 OR name = '1kg of apples';
```

<br>

## Between

Retornar tuplas onde o valor do campo esteja entre os valores informados:
```sql
SELECT * FROM sales_tb AS a
WHERE a.price BETWEEN 2.0 AND 3.0;
```

<br>

## Is Null

Retornar se for igual a nulo:
```sql
SELECT * FROM sales_tb AS a
WHERE a.price = IS NULL;
```

<br>

## Delete

Deletar onde a condição for verdadeira:
```sql
DELETE FROM sales_tb AS a
WHERE a.id = 1;
```

<br>

## Inner, Left and Right Join

Algumas formas de juntar tabelas:
```sql
--JOIN is equal to INNER JOIN
SELECT * FROM customers_tb
INNER JOIN sales_tb
ON customers_tb.id = sales_tb.customer_id;

SELECT * FROM customers_tb
LEFT JOIN sales_tb
ON customers_tb.id = sales_tb.customer_id;

SELECT * FROM customers_tb
RIGHT JOIN sales_tb
ON customers_tb.id = sales_tb.customer_id;
```

<br>

## Joins Diagram

Entendendo os *joins*:

![SQL Joins](assets/sql-joins-diagram.jpg)

<br>

## Agregate Functions

Funções de agregação:
```sql
SELECT COUNT(example_cl) FROM sales_tb;
SELECT SUM(price) FROM sales_tb;
```

## Concat

Concatenar strings:
```sql
SELECT CONCAT('hello', ' ', 'world'); --return 'hello world'

--If the PIPES_AS_CONCAT SQL mode is enabled, then || will concatenate
SELECT 'hello' || '' || 'world'; --return 'hello world'

--As of MySQL 8.0.17, this operator is deprecated; expect support for it to be removed in a future version of MySQL. Exception: Deprecation does not apply if PIPES_AS_CONCAT is enabled because, in that case, || signifies string concatentation.
```

[Mais Funções](\\../agregate-functions/README.md)

<br>

## Group By

Comumente é usado com funções agregadas para agrupar os resultados:
```sql
SELECT customer_id, COUNT(customer_id) FROM sales_tb
GROUP BY customer_id;
```

<br>

## Group By with Join and Having

Fazendo seleções mais espeficicas:
```sql
SELECT customers_tb.name AS customer_name
COUNT(sales_tb.id) AS qtt_purchased
FROM customers_tb
LEFT JOIN sales_tb
ON customers_tb.id = sales_tb.customer_id
GROUP BY customers_tb.id
--HAVING is commonly used with GROUP BY
HAVING qtt_purchased > 1;
```

