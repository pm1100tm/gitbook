# distinct, distinct on

- DISTINCT와 DISTINCT ON은 중복된 행을 제거하는 데 사용되는 PostgreSQL의 기능

## DISTINCT

- DISTINCT는 결과 집합에서 중복된 행을 제거
- 즉, 반환되는 모든 열의 값이 동일한 행이 있을 경우 중복을 제거하고 유일한 행만을 반환
- DISTINCT는 결과 집합의 모든 열을 고려하여 중복된 행을 제거합니다.

## DISTINCT ON

- DISTINCT ON은 지정된 열(또는 열의 조합)에 대해 중복된 행을 제거
- 이때, 지정된 열에 대해 가장 먼저 나오는 행 유지
- DISTINCT ON은 결과 집합에서 지정된 열에 대해 중복된 행을 제거하지만, 나머지 열에 대해서는 고려 X
- 결과 집합에는 DISTINCT ON에 지정된 열의 값이 유일하게 유지

```sql
-- Sample Table: sales
| order_id | customer_id | amount |
|----------|-------------|--------|
| 1        | 101         | 100    |
| 2        | 101         | 150    |
| 3        | 102         | 200    |
| 4        | 103         | 250    |
| 5        | 102         | 300    |

-- Using DISTINCT
SELECT DISTINCT customer_id, amount FROM sales;

-- Output
| customer_id | amount |
|-------------|--------|
| 101         | 100    |
| 101         | 150    |
| 102         | 200    |
| 103         | 250    |
| 102         | 300    |

-- Using DISTINCT ON
SELECT DISTINCT ON (customer_id) customer_id, amount FROM sales;

-- Output
| customer_id | amount |
|-------------|--------|
| 101         | 100    |
| 102         | 200    |
| 103         | 250    |
```

> PostgreSQL은 NULL을 중복으로 처리하므로 SELECT DISTINCT 절을 적용할 때 모든 NULL에 대해
> 하나의 NULL을 유지한다.

```sql
 bcolor | fcolor
--------+--------
 blue   | blue
 green  | green
 red    | red
 red    | null
 null   | red
 null   | null
(6 rows)
```
