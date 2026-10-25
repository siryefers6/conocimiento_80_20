# SQLite3 — Relaciones uno a muchos (1:N)

Relaciona un registro de una tabla con varios registros de otra mediante una clave foránea.

Ejemplo: un pedido puede tener varios embalajes.

```sql
CREATE TABLE pedidos (
    id INTEGER PRIMARY KEY,
    numero_pedido INTEGER NOT NULL
);

CREATE TABLE embalajes (
    id INTEGER PRIMARY KEY,
    pedido_id INTEGER NOT NULL,
    codigo TEXT NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id)
);
```

- `pedidos.id`: clave primaria del pedido.
    
- `embalajes.pedido_id`: clave foránea que identifica el pedido asociado.
    
- Un pedido puede tener varios embalajes; cada embalaje pertenece a un pedido.
    

Consultar la relación:

```sql
SELECT p.numero_pedido, e.codigo
FROM pedidos AS p
INNER JOIN embalajes AS e
    ON e.pedido_id = p.id;
```

Regla: coloca la clave foránea en la tabla del lado muchos. Activa `PRAGMA foreign_keys = ON` en cada conexión para aplicar las restricciones de integridad referencial.