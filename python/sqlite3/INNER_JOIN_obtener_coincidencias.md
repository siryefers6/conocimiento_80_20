# SQLite3 — INNER JOIN: obtener coincidencias

Combina filas de dos tablas cuando cumplen una condición de relación.

Ejemplo funcional:

```sql
SELECT pedidos.numero_pedido, embalajes.codigo
FROM pedidos
INNER JOIN embalajes
    ON embalajes.pedido_id = pedidos.id;
```

- `INNER JOIN`: devuelve únicamente filas con coincidencias en ambas tablas.
    
- `ON`: define la condición de unión.
    
- `pedidos.id`: clave primaria.
    
- `embalajes.pedido_id`: clave foránea.
    

Usar alias para simplificar:

```sql
SELECT p.numero_pedido, e.codigo
FROM pedidos AS p
INNER JOIN embalajes AS e
    ON e.pedido_id = p.id;
```

Combinar con filtros:

```sql
SELECT p.numero_pedido, e.codigo
FROM pedidos AS p
INNER JOIN embalajes AS e
    ON e.pedido_id = p.id
WHERE p.zona = 'Norte';
```

Regla: usa `INNER JOIN` cuando necesites datos relacionados y quieras excluir registros sin coincidencias. Utiliza condiciones `ON` que relacionen correctamente las claves.