# SQLite3 — UNION y UNION ALL: combinar resultados

Combina los resultados de dos o más consultas `SELECT` en un único conjunto de filas.

`UNION` — eliminar duplicados

```sql
SELECT zona FROM pedidos
UNION
SELECT zona FROM sucursales;
```

Devuelve las zonas sin duplicados.

`UNION ALL` — conservar duplicados

```sql
SELECT zona FROM pedidos
UNION ALL
SELECT zona FROM sucursales;
```

Devuelve todas las filas, incluidos los valores repetidos.

Reglas esenciales:

- Ambas consultas deben devolver el mismo número de columnas.
    
- Las columnas deben tener tipos de datos compatibles.
    
- `UNION` elimina duplicados y puede requerir más procesamiento.
    
- `UNION ALL` conserva duplicados y suele ser más eficiente.
    
- Coloca `ORDER BY` al final para ordenar el resultado combinado.
    

Regla: usa `UNION ALL` por defecto cuando necesites conservar todas las filas. Elige `UNION` únicamente cuando debas eliminar duplicados.