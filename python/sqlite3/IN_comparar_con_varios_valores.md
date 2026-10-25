# SQLite3 — IN: comparar con varios valores

Filtra registros cuyo valor coincida con alguno de los elementos de una lista.

```sql
SELECT *
FROM pedidos
WHERE zona IN ('Norte', 'Sur', 'Centro');
```

- `IN`: comprueba si un valor pertenece a una lista.
    
- Sustituye múltiples condiciones `OR` por una expresión más clara.
    

Combinar con `NOT IN`:

```sql
SELECT *
FROM pedidos
WHERE zona NOT IN ('Norte', 'Sur');
```

Excluye las zonas indicadas. Los registros con `zona IS NULL` tampoco aparecerán.

Usar una subconsulta:

```sql
SELECT *
FROM pedidos
WHERE numero_pedido IN (
    SELECT numero_pedido
    FROM embalajes
);
```

Selecciona pedidos cuyo número también aparece en `embalajes`.

Regla: usa `IN` para comparar un valor con varias alternativas. Evita `NOT IN` si la subconsulta puede devolver `NULL`; considera `NOT EXISTS`.