# SQLite3 — WITH: crear consultas temporales mediante CTE

Define una expresión de tabla común (CTE) para organizar una consulta compleja en bloques reutilizables dentro de una misma sentencia SQL.

Crear una CTE:

```sql
WITH pedidos_grandes AS (
    SELECT numero_pedido, cantidad_bultos
    FROM pedidos
    WHERE cantidad_bultos > 10
)
SELECT *
FROM pedidos_grandes;
```

- `WITH`: declara una CTE.
    
- `pedidos_grandes`: nombre temporal de la consulta.
    
- `AS (...)`: define la consulta que genera sus resultados.
    
- La CTE existe únicamente durante esa sentencia SQL.
    

Combinar varias CTE:

```sql
WITH resumen AS (
    SELECT zona, COUNT(*) AS total
    FROM pedidos
    GROUP BY zona
)
SELECT zona, total
FROM resumen
WHERE total > 10;
```

Regla: usa `WITH` para dividir consultas complejas en pasos legibles. Una CTE no es una tabla permanente ni garantiza que los resultados se almacenen físicamente en memoria.