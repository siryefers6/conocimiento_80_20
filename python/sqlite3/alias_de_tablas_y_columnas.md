# SQLite3 — Alias de tablas y columnas

Usa `AS` para asignar nombres temporales a tablas y columnas durante una consulta.

Alias de columnas:

```sql
SELECT
    numero_pedido AS pedido,
    cantidad_bultos AS total_bultos
FROM pedidos;
```

- `AS`: asigna un nombre temporal al resultado.
    
- Facilita la lectura de los encabezados.
    

Alias de tablas:

```sql
SELECT p.numero_pedido, e.codigo
FROM pedidos AS p
INNER JOIN embalajes AS e
    ON e.pedido_id = p.id;
```

- `p`: alias de `pedidos`.
    
- `e`: alias de `embalajes`.
    
- Simplifica consultas con varias tablas.
    

Regla: utiliza alias cortos y descriptivos. En consultas con `JOIN`, califica las columnas como `p.id` o `e.id` para evitar ambigüedades.