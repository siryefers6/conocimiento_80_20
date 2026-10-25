# SQLite3 — COUNT(): contar registros

Cuenta filas o valores no nulos para generar indicadores y reportes.

Contar todos los registros:

```sql
SELECT COUNT(*)
FROM pedidos;
```

Cuenta todas las filas, incluso si contienen valores `NULL`.

Contar valores no nulos:

```sql
SELECT COUNT(zona)
FROM pedidos;
```

Cuenta las filas donde `zona` no es `NULL`.

Contar valores únicos:

```sql
SELECT COUNT(DISTINCT zona)
FROM pedidos;
```

Cuenta las zonas distintas, excluyendo `NULL`.

Contar registros filtrados:

```sql
SELECT COUNT(*)
FROM pedidos
WHERE cantidad_bultos > 10;
```

Regla: usa `COUNT(*)` para contar filas y `COUNT(columna)` para contar valores no nulos.