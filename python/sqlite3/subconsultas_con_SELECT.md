# SQLite3 — Subconsultas con SELECT

Anida una consulta dentro de otra para utilizar su resultado como filtro o fuente de datos.

1. Subconsulta con `IN`

Busca pedidos que tengan embalajes asociados.

```sql
SELECT *
FROM pedidos
WHERE id IN (
    SELECT pedido_id
    FROM embalajes
);
```

2. Subconsulta con `EXISTS`

Comprueba si existe al menos un embalaje para cada pedido.

```sql
SELECT p.*
FROM pedidos AS p
WHERE EXISTS (
    SELECT 1
    FROM embalajes AS e
    WHERE e.pedido_id = p.id
);
```

3. Subconsulta con una agregación

Busca pedidos cuya cantidad de bultos supere el promedio general.

```sql
SELECT *
FROM pedidos
WHERE cantidad_bultos > (
    SELECT AVG(cantidad_bultos)
    FROM pedidos
);
```

- `IN`: compara valores con los resultados de una subconsulta.
    
- `EXISTS`: comprueba si una subconsulta devuelve filas.
    
- Una subconsulta escalar devuelve un único valor.
    

Regla: usa subconsultas para comparar, filtrar o comprobar relaciones. Prefiere `EXISTS` para comprobar existencia y evita `NOT IN` cuando la subconsulta pueda devolver `NULL`.