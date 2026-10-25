# SQLite3 — Restricciones: NOT NULL, UNIQUE y CHECK

Controla los datos permitidos al crear una tabla. La base de datos rechazará registros que incumplan las reglas.

```sql
CREATE TABLE embalajes (
    id INTEGER PRIMARY KEY,
    codigo TEXT NOT NULL UNIQUE,
    peso REAL CHECK (peso > 0),
    estado TEXT NOT NULL CHECK (estado IN ('pendiente', 'completo'))
);
```

- `NOT NULL`: exige un valor; no permite `NULL`.
    
- `UNIQUE`: impide valores duplicados en la columna.
    
- `CHECK`: exige que una expresión se cumpla.
    
- `NOT NULL UNIQUE`: combina ambas restricciones.
    
- `CHECK (columna IN (...))`: limita los valores permitidos.
    

Agregar restricciones al modificar datos:

```sql
-- Correcto
INSERT INTO embalajes (codigo, peso, estado)
VALUES ('ABC123', 12.5, 'pendiente');

-- Error: código duplicado
INSERT INTO embalajes (codigo, peso, estado)
VALUES ('ABC123', 8, 'completo');

-- Error: peso negativo
INSERT INTO embalajes (codigo, peso, estado)
VALUES ('XYZ789', -5, 'pendiente');
```

Regla: valida en la base de datos las reglas que siempre deben cumplirse, aunque los datos provengan de distintas partes de la aplicación.