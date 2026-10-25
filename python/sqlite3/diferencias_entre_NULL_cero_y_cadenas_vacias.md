# SQLite3 — NULL, cero y cadenas vacías

Distingue entre ausencia de valor, valor numérico cero y texto vacío. Son datos diferentes.

```sql
SELECT
    NULL IS NULL,       -- 1: no hay valor
    0 = 0,              -- 1: valor numérico cero
    '' = '';             -- 1: texto vacío
```

- `NULL`: valor desconocido, ausente o no registrado.
    
- `0`: número válido; representa una cantidad igual a cero.
    
- `''`: texto existente, pero sin caracteres.
    
- `NULL = NULL` no devuelve verdadero; devuelve `NULL`.
    
- Usa `IS NULL` o `IS NOT NULL` para comprobar valores nulos.
    

```sql
-- Buscar valores ausentes
SELECT * FROM embalajes
WHERE peso IS NULL;

-- Buscar cantidades iguales a cero
SELECT * FROM embalajes
WHERE peso = 0;

-- Buscar textos vacíos
SELECT * FROM embalajes
WHERE codigo = '';

-- Sustituir NULL por un valor predeterminado al consultar
SELECT COALESCE(peso, 0) AS peso
FROM embalajes;
```

Regla: `NULL` significa ausencia; `0` es un número y `''` es texto vacío. No los confundas ni los reemplaces automáticamente.