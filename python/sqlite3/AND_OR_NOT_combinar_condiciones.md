# SQLite3 — AND, OR, NOT: combinar condiciones

Combina o invierte condiciones dentro de `WHERE`.

- `AND`: exige que todas las condiciones sean verdaderas.
    
- `OR`: exige que al menos una condición sea verdadera.
    
- `NOT`: invierte el resultado de una condición.
    

AND — todas las condiciones

```sql
SELECT *
FROM pedidos
WHERE zona = 'Norte'
  AND cantidad_bultos > 10;
```

OR — al menos una condición

```sql
SELECT *
FROM pedidos
WHERE zona = 'Norte'
   OR zona = 'Sur';
```

NOT — negar una condición

```sql
SELECT *
FROM pedidos
WHERE NOT zona = 'Norte';
```

Combinar operadores con paréntesis

```sql
SELECT *
FROM pedidos
WHERE (zona = 'Norte' OR zona = 'Sur')
  AND cantidad_bultos > 10;
```

Los paréntesis hacen explícita la prioridad de evaluación. Sin ellos, `AND` tiene prioridad sobre `OR`.

Regla: usa paréntesis cuando combines `AND` y `OR` para evitar resultados inesperados. Recuerda que las condiciones con `NULL` requieren atención especial.