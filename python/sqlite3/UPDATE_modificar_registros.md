# SQLite3 — UPDATE: modificar registros

Modifica los valores de las filas existentes mediante `SET` y `WHERE`.

Actualizar un campo:

```sql
UPDATE pedidos
SET zona = 'Sur'
WHERE numero_pedido = 1001;
```

Actualizar varios campos:

```sql
UPDATE pedidos
SET zona = 'Norte',
    cantidad_bultos = 15
WHERE numero_pedido = 1001;
```

Actualizar mediante una condición:

```sql
UPDATE pedidos
SET cantidad_bultos = 0
WHERE zona = 'Sur';
```

- `UPDATE`: indica la tabla que modificarás.
    
- `SET`: define los nuevos valores.
    
- `WHERE`: selecciona las filas que modificarás.
    

Regla crítica: verifica el `WHERE` antes de ejecutar. Sin esa cláusula, actualizarás todas las filas.

```sql
BEGIN;

UPDATE pedidos
SET zona = 'Sur'
WHERE numero_pedido = 1001;

SELECT * FROM pedidos WHERE numero_pedido = 1001;

COMMIT;
```

Usa `ROLLBACK` en lugar de `COMMIT` si necesitas cancelar la transacción antes de confirmarla.