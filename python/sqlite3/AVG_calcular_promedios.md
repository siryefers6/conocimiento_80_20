# SQLite3 — AVG(): calcular promedios

Calcula la media aritmética de los valores numéricos de una columna.

Calcular el promedio:

```sql
SELECT AVG(cantidad_bultos) AS promedio
FROM pedidos;
```

Calcular el promedio con una condición:

```sql
SELECT AVG(cantidad_bultos) AS promedio
FROM pedidos
WHERE zona = 'Norte';
```

Calcular promedios por categoría:

```sql
SELECT zona, AVG(cantidad_bultos) AS promedio
FROM pedidos
GROUP BY zona;
```

- `AVG()`: ignora valores `NULL`.
    
- `AS`: asigna un nombre al resultado.
    
- Devuelve `NULL` si no existen valores numéricos válidos.
    

Regla: usa `AVG()` para calcular promedios. No confundas la media con la mediana.