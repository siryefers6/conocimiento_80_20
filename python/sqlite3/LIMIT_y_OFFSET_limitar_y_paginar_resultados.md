# SQLite3 — LIMIT y OFFSET: limitar y paginar resultados

Controla cuántos registros devuelve una consulta y desde qué posición comienza.

```sql
SELECT *
FROM pedidos
ORDER BY numero_pedido
LIMIT 10;
```

- `LIMIT`: devuelve como máximo la cantidad indicada.
    
- `OFFSET`: omite los primeros registros.
    
- `ORDER BY`: establece un orden estable para paginar.
    

Paginar resultados:

```sql
SELECT *
FROM pedidos
ORDER BY numero_pedido
LIMIT 10 OFFSET 20;
```

Devuelve 10 registros después de omitir los primeros 20.

Calcular el desplazamiento:

```python
limite = 10
pagina = 3
offset = (pagina - 1) * limite  # 20
```

Regla: combina `LIMIT` y `OFFSET` para paginar tablas. Ordena por una columna única para evitar resultados inconsistentes entre páginas.