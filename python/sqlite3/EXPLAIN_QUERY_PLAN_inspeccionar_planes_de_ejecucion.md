# SQLite3 — EXPLAIN QUERY PLAN: inspeccionar planes de ejecución

Muestra cómo SQLite planea ejecutar una consulta. Úsalo para detectar búsquedas costosas y comprobar el uso de índices.

Inspeccionar una consulta:

```sql
EXPLAIN QUERY PLAN
SELECT *
FROM pedidos
WHERE zona = 'Norte';
```

Interpretar resultados habituales:

- `SCAN pedidos`: recorre la tabla.
    
- `SEARCH pedidos USING INDEX ...`: utiliza un índice para buscar registros.
    
- `USING COVERING INDEX`: obtiene los datos necesarios directamente del índice.
    
- `USE TEMP B-TREE FOR ORDER BY`: utiliza una estructura temporal para ordenar.
    

Comparar antes y después de crear un índice:

```sql
CREATE INDEX idx_pedidos_zona
ON pedidos(zona);

EXPLAIN QUERY PLAN
SELECT *
FROM pedidos
WHERE zona = 'Norte';
```

- Sin índice adecuado, SQLite puede recorrer toda la tabla.
    
- Con un índice adecuado, puede buscar directamente las coincidencias.
    
- El optimizador puede decidir que un recorrido completo es más eficiente, especialmente en tablas pequeñas.
    

Regla: utiliza `EXPLAIN QUERY PLAN` para diagnosticar consultas lentas. Interpreta el plan junto con el tamaño de la tabla y la frecuencia de ejecución; no asumas que todo `SCAN` es un problema.