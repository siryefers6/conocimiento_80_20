# SQLite3 — BEGIN, COMMIT y ROLLBACK: controlar transacciones

Agrupa operaciones SQL en una transacción para confirmar todos los cambios o cancelarlos.

- `BEGIN`: inicia una transacción.
    
- `COMMIT`: confirma los cambios.
    
- `ROLLBACK`: cancela los cambios pendientes.
    

Ejemplo funcional:

```sql
BEGIN;

UPDATE pedidos
SET zona = 'Sur'
WHERE numero_pedido = 1001;

SELECT *
FROM pedidos
WHERE numero_pedido = 1001;

COMMIT;
```

Si detectas un error antes de confirmar, ejecuta `ROLLBACK` en lugar de `COMMIT`.

Regla: confirma únicamente después de validar los resultados.

- Si una operación falla, cancela la transacción cuando corresponda.
    
- No ejecutes `COMMIT` si necesitas deshacer los cambios.
    
- SQLite inicia transacciones automáticamente para operaciones individuales; usa transacciones explícitas para agrupar varias operaciones relacionadas.