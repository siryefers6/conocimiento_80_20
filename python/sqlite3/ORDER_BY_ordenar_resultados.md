# SQLite3 — ORDER BY: ordenar resultados

Ordena los registros según una o varias columnas.

```sql
SELECT numero_pedido, cantidad_bultos
FROM pedidos
ORDER BY cantidad_bultos DESC;
```

- `ORDER BY`: define el criterio de ordenamiento.
    
- `ASC`: orden ascendente (predeterminado).
    
- `DESC`: orden descendente.
    

Ordenar por varias columnas:

```sql
SELECT numero_pedido, zona
FROM pedidos
ORDER BY zona ASC, numero_pedido DESC;
```

Primero ordena por zona y después por número de pedido dentro de cada zona.

Regla: usa `ORDER BY` para controlar el orden de los resultados. Sin esta cláusula, no asumas un orden específico.