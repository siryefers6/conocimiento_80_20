# SQLite3 — HAVING: filtrar grupos

Filtra los grupos después de aplicar `GROUP BY`, utilizando condiciones sobre agregaciones.

Filtrar grupos por cantidad:

```sql
SELECT zona, COUNT(*) AS total
FROM pedidos
GROUP BY zona
HAVING COUNT(*) > 10;
```

Devuelve las zonas que tienen más de 10 pedidos.

Filtrar grupos por suma:

```sql
SELECT zona, SUM(cantidad_bultos) AS total_bultos
FROM pedidos
GROUP BY zona
HAVING SUM(cantidad_bultos) >= 100;
```

Combinar `WHERE` y `HAVING`:

```sql
SELECT zona, COUNT(*) AS total
FROM pedidos
WHERE cantidad_bultos > 0
GROUP BY zona
HAVING COUNT(*) >= 5;
```

- `WHERE`: filtra registros antes de agrupar.
    
- `GROUP BY`: agrupa los registros.
    
- `HAVING`: filtra los grupos resultantes.
    

Regla: usa `WHERE` para condiciones sobre filas y `HAVING` para condiciones sobre grupos.