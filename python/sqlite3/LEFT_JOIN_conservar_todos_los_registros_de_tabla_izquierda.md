# SQLite3 — LEFT JOIN: conservar todos los registros de la tabla izquierda

Devuelve todas las filas de la tabla izquierda y las coincidencias de la tabla derecha. Si no existe coincidencia, devuelve `NULL` en las columnas de la derecha.

Ejemplo funcional:

```sql
SELECT p.numero_pedido, e.codigo
FROM pedidos AS p
LEFT JOIN embalajes AS e
    ON e.pedido_id = p.id;
```

- `LEFT JOIN`: conserva todos los pedidos, tengan embalajes o no.
    
- `ON`: define cómo relacionar ambas tablas.
    
- `NULL`: indica que no existe una coincidencia en la tabla derecha.
    

Encontrar registros sin coincidencias:

```sql
SELECT p.numero_pedido
FROM pedidos AS p
LEFT JOIN embalajes AS e
    ON e.pedido_id = p.id
WHERE e.id IS NULL;
```

Devuelve los pedidos que no tienen embalajes asociados.

Regla: coloca a la izquierda la tabla cuyos registros necesitas conservar. Para detectar registros sin relación, filtra por una clave no nula de la tabla derecha con `IS NULL`.