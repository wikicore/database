# Funções

## Pré-requisitos
Conhecimentos necessários:
* SQL
* Funções
* Padrões matemáticos (opcional) (ajudará no entendimento do conteúdo com pouco texto)

## Syntax

```sql
--count values
SELECT COUNT(column_name);

--sum values
SELECT SUM(column_name);

--average
SELECT AVG(column_name);

--max value
SELECT MAX(column_name);

--min value
SELECT MIN(column_name);

--round value
SELECT ROUND(6.855, 2); --return 6.86

--cast this as that (convert the type)
SELECT CAST(expression AS datatype);
SELECT CAST(product.id AS VARCHAR(5));

--returns the string with all prefixes or suffixes removed
SELECT TRIM('  hello   '); --return 'hello'
SELECT TRIM(LEADING 'x' FROM 'xxxhelloxxx'); --return 'helloxxx'
SELECT TRIM(BOTH 'x' FROM 'xxxhelloxxx'); --return 'hello'
SELECT TRIM(TRAILING 'xyz' FROM 'helloxxyz'); --return 'hellox'

--returns string starting in x and ending in y
SELECT SUBSTR('hello world', 7); --return 'world'
SELECT SUBSTR('hello world', 7, 9); --return 'wor'
SELECT SUBSTR('hello world', -3); --return 'rld'

--text replace
SELECT REPLACE('hello word', 'word', 'friend'); --return 'hello friend'

--extracts a part from a given date
SELECT EXTRACT(part FROM some_date);
SELECT EXTRACT(SECOND FROM '2021-10-02 19:58:55'); --return '55'

--return modified string
SELECT LPAD(some_str, length_str, left_str);
SELECT LPAD('5', 2, 0); --return '05'
SELECT LPAD('world', 17, 'hello '); --return 'hello hello world'

--returning the first non-null value in a list, if all is null then returns null
SELECT COALESCE(NULL, NULL, 10); --return 10
SELECT COALESCE(NULL, NULL, NULL); --return null
```

<br>

## [Documentação completa](https://dev.mysql.com/doc/refman/8.0/en/functions.html)