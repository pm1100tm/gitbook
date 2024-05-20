# CTE

## CTE(Common Table Expression, 공통 테이블 표현식)

- 쿼리 내에서 임시 결과 집합을 만든다.
- 복잡한 쿼리를 더 작고 재사용 가능한 부분으로 나누어 가독성을 높인다.

## 장점

- 복잡한 쿼리의 가독성 개선
- 참조하는 쿼리의 재귀 쿼리 만들 수 있음
- window 함수와 사용하여 초기 결과 집합을 만들고, 다른 select문을 사용하여 결과 집합 추가 처리

## 기본 구문

```sql
WITH cte_name (col1, col2, ...) AS (
    -- CTE Query
    SELECT ...
)

SELECT ... FROM cte_name;
```

WITH clause + CTE name + Column List (optional) + AS keyword + CTE query

## 예제

```sql
-- sample 1
WITH action_films AS (
  SELECT
    f.title,
    f.length
  FROM
    film f
    INNER JOIN film_category fc USING (film_id)
    INNER JOIN category c USING(category_id)
  WHERE
    c.name = 'Action'
)

SELECT * FROM action_films;


-- sample 2
WITH cte_rental AS (
  SELECT
    staff_id,
    COUNT(rental_id) rental_count
  FROM
    rental
  GROUP BY
    staff_id
)

SELECT
  s.staff_id,
  first_name,
  last_name,
  rental_count
FROM
  staff s
  INNER JOIN cte_rental USING (staff_id);


-- sample 3
WITH film_stats AS (
    -- CTE 1: Calculate film statistics
    SELECT
        AVG(rental_rate) AS avg_rental_rate,
        MAX(length) AS max_length,
        MIN(length) AS min_length
    FROM film
),
customer_stats AS (
    -- CTE 2: Calculate customer statistics
    SELECT
        COUNT(DISTINCT customer_id) AS total_customers,
        SUM(amount) AS total_payments
    FROM payment
)

-- Main query using the CTEs
SELECT
    ROUND((SELECT avg_rental_rate FROM film_stats), 2) AS avg_film_rental_rate,
    (SELECT max_length FROM film_stats) AS max_film_length,
    (SELECT min_length FROM film_stats) AS min_film_length,
    (SELECT total_customers FROM customer_stats) AS total_customers,
    (SELECT total_payments FROM customer_stats) AS total_payments;
```

## Recursive CTE

```sql
CREATE TABLE employees (
  employee_id SERIAL PRIMARY KEY,
  full_name VARCHAR NOT NULL,
  manager_id INT
);

INSERT INTO employees (employee_id, full_name, manager_id)
VALUES
  (1, 'Michael North', NULL),
  (2, 'Megan Berry', 1),
  (3, 'Sarah Berry', 1),
  (4, 'Zoe Black', 1),
  (5, 'Tim James', 1),
  (6, 'Bella Tucker', 2),
  (7, 'Ryan Metcalfe', 2),
  (8, 'Max Mills', 2),
  (9, 'Benjamin Glover', 2),
  (10, 'Carolyn Henderson', 3),
  (11, 'Nicola Kelly', 3),
  (12, 'Alexandra Climo', 3),
  (13, 'Dominic King', 3),
  (14, 'Leonard Gray', 4),
  (15, 'Eric Rampling', 4),
  (16, 'Piers Paige', 7),
  (17, 'Ryan Henderson', 7),
  (18, 'Frank Tucker', 8),
  (19, 'Nathan Ferguson', 8),
  (20, 'Kevin Rampling', 8);
```

어떤 직원의 매니저인 직원의 정보를 취득

```sql
WITH RECURSIVE subordinates AS (
  SELECT
    employee_id,
    manager_id,
    full_name
  FROM
    employees
  WHERE
    employee_id = 2

  UNION

  SELECT
    e.employee_id,
    e.manager_id,
    e.full_name
  FROM
    employees e
    INNER JOIN subordinates s ON s.employee_id = e.manager_id
)

SELECT * FROM subordinates;
```
