# SQLite3 — date(), datetime() y strftime(): trabajar con fechas

Convierte, calcula y formatea fechas utilizando las funciones integradas de SQLite 3.

1. `date()` — obtener una fecha

```sql
SELECT date('now') AS fecha_actual;
```

Devuelve la fecha en formato `YYYY-MM-DD`.

2. `datetime()` — obtener fecha y hora

```sql
SELECT datetime('now', 'localtime') AS fecha_hora;
```

Devuelve la fecha y hora local del sistema donde se ejecuta SQLite.

3. `strftime()` — extraer y formatear componentes

```sql
SELECT strftime('%Y-%m', fecha_registro) AS mes
FROM pedidos;
```

Devuelve el año y el mes, por ejemplo, `2026-10`.

| Formato | Resultado                      |
| ------- | ------------------------------ |
| `%Y`    | Año                            |
| `%m`    | Mes                            |
| `%d`    | Día                            |
| `%H`    | Hora                           |
| `%M`    | Minuto                         |
| `%S`    | Segundo                        |
| `%w`    | Día de la semana (0 = domingo) |

Filtrar registros por fecha:

```sql
SELECT *
FROM pedidos
WHERE fecha_registro >= '2026-10-01'
  AND fecha_registro < '2026-11-01';
```

Este intervalo incluye todo octubre si las fechas están almacenadas en un formato ISO 8601 comparable.

Regla: almacena fechas en formatos consistentes, preferiblemente ISO 8601. Usa `date()` para fechas, `datetime()` para fecha y hora, y `strftime()` para extraer componentes o agrupar reportes por período.