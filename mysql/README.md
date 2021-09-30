# Comandos Básicos Com MySQL

## Pre-requisitos
Conhecimentos necessários:
* SQL
* Diagrama de Venn (opcional)

<br>

## Instalando o Banco de Dados

[Download MySQL](https://dev.mysql.com/downloads/mysql/)

Durante instalação selecionar:

- [x] Server
- [x] Workbench (opcional)

<br>

## Create Database

Criar novo banco de dados:
```sql
CREATE DATABASE example_db;
```

<br>

## Drop Database

Exclui um banco de dados:
```sql
DROP DATABASE example_db;
```

<br>

## Use Database

Seleciona um banco de dados:
```sql
USE DATABASE example_db;
```

<br>

## Create Table

Cria uma tabela:
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

Apaga um tabela:
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

Insere dados em uma tabela:
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

Seleciona tudo da tabela *sales_tb*:
```sql
SELECT * FROM sales_tb;
```

<br>

## Specific Column and Limit

Especifica uma coluna e limita o número de linhas:
```sql
SELECT name FROM sales_tb LIMIT 2;
```

<br>

## As, Alias Columns

Retorna a coluna com outro nome:
```sql
SELECT name AS 'Product', price AS 'Price' FROM sales_tb;
```

<br>

## Order By

Ordena a coluna:
```sql
SELECT * FROM sales_tb ORDER BY name;
```

<br>

## Distinct Select

Não retorna dados repetidos:
```sql
SELECT DISTINCT name FROM sales_tb;
```

<br>

## Update and Where

Atualiza um campo seguindo uma condição:
```sql
UPDATE sales_tb
SET price = 3.49
WHERE id = 1;
```

<br>

## Comparators

Retorna tuplas onde a condição é verdadeira:
```sql
SELECT * FROM sales_tb
WHERE price > 4.0;
```

<br>

## Like String Filter

Retorna se a string conter os caracteres:
```sql
SELECT * FROM sales_tb
WHERE name LIKE '%an%';
--may contain characters to the left -> %an
--an% <- may contain characters to the right
```

<br>

## Or and And

Retorna tuplas onde a condição for verdadeira:
```sql
SELECT * FROM sales_tb
WHERE price = 3.99 OR name = '1kg of apples';
```

<br>

## Between

Retorna tuplas onde o campo esta entre os valores informados:
```sql
SELECT * FROM sales_tb
WHERE price BETWEEN 2.0 AND 3.0;
```

<br>

## Is Null

Retorna se for igual a nulo:
```sql
SELECT * FROM sales_tb
WHERE price = IS NULL;
```

<br>

## Delete

Deleta onde a condição for verdadeira:
```sql
DELETE FROM sales_tb WHERE id = 1;
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
SELECT AVG(price) FROM sales_tb;
SELECT SUM(price) FROM sales_tb;
```

<br>

## Group By

Usado com funções agregadas para agrupar os resultados:
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

