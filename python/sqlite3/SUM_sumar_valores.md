# SQLite3 — SUM(): sumar valores

Calcula la suma de los valores numéricos de una columna.

Sumar todos los valores:

```sql
SELECT SUM(cantidad_bultos)
FROM pedidos;
```

Sumar con una condición:

```sql
SELECT SUM(cantidad_bultos)
FROM pedidos
WHERE zona = 'Norte';
```

Agrupar sumas por categoría:

```sql
SELECT zona, SUM(cantidad_bultos) AS total_bultos
FROM pedidos
GROUP BY zona;
```

- `SUM()`: suma los valores no nulos.
    
- `AS`: asigna un alias al resultado.
    
- Si no existen valores no nulos, devuelve `NULL`.
    

Regla: usa `SUM()` para calcular totales. Aplica `COALESCE(SUM(columna), 0)` cuando necesites obtener `0` en lugar de `NULL`.