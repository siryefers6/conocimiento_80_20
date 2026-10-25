# SQLite3 — FOREIGN KEY: relacionar tablas

Define una clave foránea para vincular registros y mantener la integridad referencial entre tablas.

Crear una relación:

```sql
CREATE TABLE embalajes (
    id INTEGER PRIMARY KEY,
    pedido_id INTEGER NOT NULL,
    codigo TEXT NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id)
);
```

- `FOREIGN KEY`: declara la columna que establece la relación.
    
- `REFERENCES`: indica la tabla y la clave de destino.
    
- `pedido_id`: debe corresponder a un `id` existente en `pedidos`.
    

Activar claves foráneas:

```sql
PRAGMA foreign_keys = ON;
```

Actívalas en cada conexión a SQLite, antes de iniciar una transacción.

Eliminar en cascada:

```sql
FOREIGN KEY (pedido_id)
REFERENCES pedidos(id)
ON DELETE CASCADE
```

Al eliminar un pedido, SQLite también eliminará sus embalajes relacionados.

Regla: usa `FOREIGN KEY` para impedir referencias inválidas. Aplica `ON DELETE CASCADE` únicamente cuando quieras que los registros dependientes se eliminen automáticamente.