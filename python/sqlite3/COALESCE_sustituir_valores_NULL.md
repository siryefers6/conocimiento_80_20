# SQLite3 — COALESCE(): sustituir valores NULL

Devuelve el primer argumento que no sea `NULL`. Úsalo para establecer valores predeterminados en consultas.

Sustituir `NULL` por cero:

```sql
SELECT
    numero_pedido,
    COALESCE(cantidad_bultos, 0) AS cantidad_bultos
FROM pedidos;
```

Combinar varias alternativas:

```sql
SELECT COALESCE(zona, 'Sin zona', 'Desconocida')
FROM pedidos;
```

Devuelve `zona` si no es `NULL`; de lo contrario, devuelve `'Sin zona'`. El tercer argumento no se utilizará porque el segundo nunca es `NULL`.

Aplicar a una suma:

```sql
SELECT COALESCE(SUM(cantidad_bultos), 0) AS total
FROM pedidos;
```

Devuelve `0` si la suma resulta `NULL`, por ejemplo, cuando no hay valores válidos.

- `COALESCE()`: evalúa los argumentos de izquierda a derecha.
    
- Devuelve el primer valor no nulo.
    
- No modifica los datos almacenados.
    

Regla: usa `COALESCE()` para controlar valores ausentes en resultados, cálculos e informes.