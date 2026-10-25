# SQLite3 — IS NULL e IS NOT NULL: comprobar valores nulos

Comprueba si una columna contiene `NULL` o un valor distinto de `NULL`.

Buscar valores nulos:

```sql
SELECT *
FROM embalajes
WHERE peso IS NULL;
```

Buscar valores no nulos:

```sql
SELECT *
FROM embalajes
WHERE peso IS NOT NULL;
```

- `IS NULL`: identifica valores ausentes o desconocidos.
    
- `IS NOT NULL`: identifica valores presentes, incluidos `0` y `''` (texto vacío).
    

Regla: no uses `= NULL` ni `!= NULL`. Utiliza `IS NULL` o `IS NOT NULL` para comprobar valores nulos.