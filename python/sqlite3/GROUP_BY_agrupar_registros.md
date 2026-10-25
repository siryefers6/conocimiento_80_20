# SQLite3 — GROUP BY: agrupar registros

Agrupa filas que comparten valores para calcular indicadores por categoría.

Contar registros por zona:

```sql
SELECT zona, COUNT(*) AS total
FROM pedidos
GROUP BY zona;
```

Calcular totales por categoría:

```sql
SELECT zona, SUM(cantidad_bultos) AS total_bultos
FROM pedidos
GROUP BY zona;
```

- `GROUP BY`: agrupa filas con valores iguales.
    
- `COUNT()`, `SUM()`, `AVG()`, `MIN()` y `MAX()`: calculan resultados por grupo.
    
- `AS`: asigna un nombre al resultado.
    

Agrupar por varias columnas:

```sql
SELECT zona, cantidad_bultos, COUNT(*) AS total
FROM pedidos
GROUP BY zona, cantidad_bultos;
```

Regla: selecciona únicamente columnas agrupadas y agregaciones, salvo que conozcas las particularidades de SQLite. Usa `WHERE` para filtrar filas antes de agrupar.