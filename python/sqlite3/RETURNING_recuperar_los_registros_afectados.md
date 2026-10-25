# SQLite3 — RETURNING: recuperar los registros afectados

Devuelve las columnas de las filas insertadas, modificadas o eliminadas. Disponible desde SQLite 3.35.0.

Recuperar un registro insertado:

```sql
INSERT INTO pedidos (numero_pedido, cantidad_bultos, zona)
VALUES (1001, 12, 'Norte')
RETURNING numero_pedido, zona;
```

Recuperar registros modificados:

```sql
UPDATE pedidos
SET zona = 'Sur'
WHERE numero_pedido = 1001
RETURNING numero_pedido, zona;
```

Recuperar registros eliminados:

```sql
DELETE FROM pedidos
WHERE cantidad_bultos = 0
RETURNING numero_pedido, zona;
```

- `RETURNING *`: devuelve todas las columnas afectadas.
    
- `RETURNING columna`: devuelve únicamente las columnas indicadas.
    
- Si ninguna fila coincide con `WHERE`, no devuelve filas.
    

Regla: usa `RETURNING` para confirmar qué datos afectó una operación sin ejecutar una consulta `SELECT` adicional. No lo confundas con una transacción: no sustituye a `COMMIT` ni a `ROLLBACK`.