#### Separating first_name and last_name (1 to 2 columns)
> [!WARNING] COMMA ' , '
>"It should not be used between the table name and its columns, or after the last column in a list."
>

```sql
SELECT 
	first_name, 
	SUBSTRING( first_name,  1, 5),
	POSITION( ' ' IN first_name),
	SUBSTRING(first_name, 0, POSITION(' ' IN first_name)+1) AS namae, 
	SUBSTRING(first_name, POSITION(' ' IN first_name)+1) AS cognom,
	TRIM(SUBSTRING(first_name, POSITION(' ' IN first_name))) AS trimmed_name
FROM actor;
```
#### 'BETWEEN' & '><'
With BETWEEN the number is included. 
With <> the number is not included at least we add = like this ( >=, <= ).
```SQL
SELECT * 
FROM actor
WHERE actor_id BETWEEN 100 AND 200 
WHERE actor_id > 100 AND actor_id < 200
ORDER BY actor_id desc;
```
##### COUNT | MIN | MAX | AVG |
With COUNT, we can see the total number of actors (one num, not list)
WIth MIN, we can see the minimum replacement cost
With MAX, we can see the maximum replacement cost
With AVG, we can see average of the column
To round the AVG number:  
- `ROUND( AVG(replacement_cost)) AS average_cost`
```SQL
SELECT COUNT(*) AS total_films, // 1000
MIN(replacement_cost) AS min_replace_cost, // 9.99
MAX(replacement_cost) AS max_replace_cost, // 29.99
AVG(replacement_cost) AS average_cost // 19.9840000000000000
SUM(replacement_cost) / count(*) AS avg_manual
FROM film;
```

With the Keyword OR we can have both results 1 and 7
If we use AND, wrong query, it is like city_id 1 and 7= false.
But with BETWEEN ... AND ... it is correct!
```SQL
SELECT * from address
WHERE city_id = 1 OR city_id = 7 
WHERE city_id = 1 AND city_id = 7 // WRONG WAY! 
WHERE city_id BETWEEN 1 AND 7 
```
#### GROUP BY
```SQL
SELECT COUNT(*) 
FROM address
WHERE city_id = 1 OR city_id = 7 
GROUP BY city_id;

SELECT
COUNT(*), replacement_cost
FROM film
GROUP BY replacement_cost
--ORDER BY replacement_cost ASC
ORDER BY count(*) ASC;
```
 ##### GROUP BY WITH OTHERS FUNCTIONS
 +1 has to be in the SUBSTRING and makes a jump in the @. 
```SQL
SELECT COUNT(*), SUBSTRING(email, POSITION('@' IN email) + 1) AS domain
FROM CUSTOMER
GROUP BY SUBSTRING(email, POSITION('@' IN email) + 1)
HAVING COUNT(*) > 1
ORDER BY SUBSTRING(email, POSITION('@' IN email) + 1) ASC;
```
##### SUBQUERIES
SELECT * FROM Table A
WHERE (Sub Query from Table B)
#### TERMINOLOGY
1. DDL Data Definition Language
	- Create, Alter, Drop, Truncate
2. DML Data Manipulation Language
	- Insert, Delete, Update
3. TCL Transaction Control Language
	-  Commit, Rollback
4. DQL Data Query Language
	- Select
#### AGGREGATE FUNCTION & FILTERING
##### Aggregate functions
1. COUNT
2. SUM
3. MAX
4. MIN
5. GROUP BY
6. HAVING
7. ORDER BY
##### Filtering data
1. LIKE
2. IN
3. IS NULL
4. IS NOT NULL
5. WHERE
6. AND
7. OR
8. BETWEEN
##### SELECT STRUCTURE
SELECT *, campos, alias, funciones
WHERE condición, condiciones, and, or, in, like
JOINS
GROUP BY campo agrupador, ALL
HAVING condición
ORDER BY expresión, ASC, DESC
LIMIT valor, ALL
OFFSET punto de inicio
#### HAVING
```SQL
SELECT
COUNT(*),
replacement_cost
FROM film
GROUP BY replacement_cost
--HAVING COUNT(*) > 40
HAVING COUNT(*) BETWEEN 40 AND 45
--ORDER BY replacement_cost ASC
ORDER BY count(*) ASC;
```
#### DISTINCT
GIve minimum one instance of each film of the list. Not-repeated ones.
`SELECT DISTINCT * FROM film;`

####

####

