# SQLite3 — BETWEEN: filtrar rangos

Selecciona registros cuyo valor esté dentro de un rango inclusivo.

```sql
SELECT *
FROM pedidos
WHERE cantidad_bultos BETWEEN 10 AND 20;
```

Incluye valores desde `10` hasta `20`, ambos inclusive.

Filtrar fechas:

```sql
SELECT *
FROM pedidos
WHERE fecha_registro BETWEEN '2026-10-01' AND '2026-10-31';
```

Funciona si las fechas están almacenadas en un formato comparable, como `YYYY-MM-DD`.

Equivalencia:

```sql
WHERE cantidad_bultos >= 10
  AND cantidad_bultos <= 20;
```

Regla: usa `BETWEEN` para rangos inclusivos. Para fechas con hora, prefiere un intervalo semiabierto (`>= inicio AND < día_siguiente`) para incluir todo el último día.