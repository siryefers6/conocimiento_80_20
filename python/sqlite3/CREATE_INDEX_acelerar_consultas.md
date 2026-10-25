# SQLite3 — CREATE INDEX: acelerar consultas

Crea un índice para localizar registros más rápido al filtrar, ordenar o relacionar tablas.

Crear un índice:

```sql
CREATE INDEX idx_pedidos_zona
ON pedidos(zona);
```

Aprovecharlo en una consulta:

```sql
SELECT *
FROM pedidos
WHERE zona = 'Norte';
```

Crear un índice compuesto:

```sql
CREATE INDEX idx_pedidos_zona_fecha
ON pedidos(zona, fecha_registro);
```

Es útil para consultas que filtran por `zona` y después por `fecha_registro`. El orden de las columnas importa.

Verificar el uso del índice:

```sql
EXPLAIN QUERY PLAN
SELECT *
FROM pedidos
WHERE zona = 'Norte';
```

- `CREATE INDEX`: crea el índice.
    
- `EXPLAIN QUERY PLAN`: muestra cómo SQLite planea ejecutar la consulta.
    
- Los índices aceleran ciertas lecturas, pero consumen espacio y pueden ralentizar escrituras.
    

Regla: crea índices sobre columnas utilizadas frecuentemente en `WHERE`, `JOIN` y `ORDER BY`. Verifica el plan de ejecución y evita indexar indiscriminadamente.