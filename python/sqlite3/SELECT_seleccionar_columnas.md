# SQLite3 — SELECT: seleccionar columnas

```sql
SELECT columna1, columna2
FROM tabla;
```

- `SELECT`: indica las columnas que quieres consultar.
    
- `FROM`: especifica la tabla de origen.
    
- `*`: selecciona todas las columnas. Evítalo si solo necesitas algunas.
    

Ejemplo:

```sql
SELECT numero_pedido, zona
FROM pedidos;
```

Consulta todas las columnas:

```sql
SELECT *
FROM pedidos;
```

Regla: selecciona únicamente las columnas necesarias. Reduce datos innecesarios y mejora la claridad.