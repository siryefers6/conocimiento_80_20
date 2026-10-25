# SQLite3 — DISTINCT: eliminar duplicados

Devuelve únicamente los valores únicos de las columnas seleccionadas.

```sql
SELECT DISTINCT zona
FROM pedidos;
```

- `DISTINCT`: elimina filas duplicadas del resultado.
    
- Se aplica a la combinación de todas las columnas seleccionadas.
    

Combinar columnas:

```sql
SELECT DISTINCT zona, cantidad_bultos
FROM pedidos;
```

Devuelve combinaciones únicas de zona y cantidad de bultos.

Contar valores únicos:

```sql
SELECT COUNT(DISTINCT zona)
FROM pedidos;
```

Cuenta cuántas zonas distintas existen, excluyendo los valores `NULL`.

Regla: usa `DISTINCT` para identificar valores o combinaciones únicas sin modificar los datos almacenados.