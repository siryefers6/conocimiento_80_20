# SQLite3 — PRIMARY KEY: identificar registros

Define una columna o conjunto de columnas que identifiquen cada fila de forma única.

Crear una tabla con clave primaria:

```sql
CREATE TABLE pedidos (
    id INTEGER PRIMARY KEY,
    numero_pedido INTEGER NOT NULL,
    zona TEXT
);
```

- `PRIMARY KEY`: garantiza la unicidad de la clave.
    
- `INTEGER PRIMARY KEY`: en una tabla normal de SQLite, actúa como alias de `ROWID`.
    
- `NOT NULL`: exige que el campo tenga un valor.
    

Clave primaria compuesta:

```sql
CREATE TABLE detalle_pedido (
    pedido_id INTEGER,
    embalaje_id INTEGER,
    PRIMARY KEY (pedido_id, embalaje_id)
);
```

La combinación de ambas columnas debe ser única.

Consultar por clave primaria:

```sql
SELECT *
FROM pedidos
WHERE id = 1;
```

Regla: define una clave primaria para identificar cada registro. Usa claves compuestas cuando la identidad dependa de varios campos.