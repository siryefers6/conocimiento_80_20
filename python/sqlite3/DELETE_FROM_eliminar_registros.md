# SQLite3 — DELETE FROM: eliminar registros

Elimina filas de una tabla mediante una condición `WHERE`.

Eliminar un registro específico:

```sql
DELETE FROM pedidos
WHERE numero_pedido = 1001;
```

Eliminar varios registros según una condición:

```sql
DELETE FROM pedidos
WHERE cantidad_bultos = 0;
```

Eliminar todos los registros:

```sql
DELETE FROM pedidos;
```

Esta última consulta conserva la tabla, pero elimina todas sus filas.

- `DELETE FROM`: indica la tabla de origen.
    
- `WHERE`: selecciona las filas que eliminarás.
    

Regla crítica: verifica el `WHERE` antes de ejecutar. Sin esa cláusula, eliminarás todos los registros.

Usa una transacción para validar antes de confirmar:

```sql
BEGIN;

DELETE FROM pedidos
WHERE numero_pedido = 1001;

SELECT * FROM pedidos
WHERE numero_pedido = 1001;

-- Si el resultado es correcto:
COMMIT;

-- Si necesitas cancelar, ejecuta ROLLBACK
-- en lugar de COMMIT.
```