# SQLite3 — MIN() y MAX(): obtener extremos

Obtén el valor mínimo y máximo de una columna.

Consultar los extremos:

```sql
SELECT
    MIN(cantidad_bultos) AS minimo,
    MAX(cantidad_bultos) AS maximo
FROM pedidos;
```

Filtrar antes de calcular:

```sql
SELECT MAX(cantidad_bultos) AS maximo
FROM pedidos
WHERE zona = 'Norte';
```

Obtener extremos por categoría:

```sql
SELECT
    zona,
    MIN(cantidad_bultos) AS minimo,
    MAX(cantidad_bultos) AS maximo
FROM pedidos
GROUP BY zona;
```

- `MIN()`: devuelve el valor menor.
    
- `MAX()`: devuelve el valor mayor.
    
- Ambas funciones ignoran los valores `NULL`.
    
- Si no hay valores válidos, devuelven `NULL`.
    

Regla: usa `MIN()` y `MAX()` para identificar extremos numéricos, fechas más antiguas o recientes y valores alfabéticos.