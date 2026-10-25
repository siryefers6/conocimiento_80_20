# `astimezone()` y `replace(tzinfo=...)` — Conversión y asignación horaria

**Python 3.14 · Biblioteca estándar**

Distingue la conversión de un instante entre zonas horarias de la asignación de información horaria sin cambiar los componentes del reloj.

## `astimezone()` — Convertir entre zonas

Conserva el instante y ajusta la hora local según la zona de destino.

```python
from datetime import UTC, datetime
from zoneinfo import ZoneInfo

fecha_utc = datetime(2026, 10, 24, 12, 0, tzinfo=UTC)
fecha_chile = fecha_utc.astimezone(ZoneInfo("America/Santiago"))

print(fecha_chile)
```

## `replace(tzinfo=...)` — Asignar una zona

Conserva los componentes de fecha y hora, pero cambia la información horaria asociada.

```python
from datetime import datetime
from zoneinfo import ZoneInfo

fecha = datetime(2026, 10, 24, 9, 0)
fecha_aware = fecha.replace(tzinfo=ZoneInfo("America/Santiago"))

print(fecha_aware)
```

Usa esta operación cuando la hora original represente una hora local de la zona indicada y todavía no tenga zona horaria.

## Diferencia fundamental

| Operación              | Hora del reloj | Instante                        |
| ---------------------- | -------------- | ------------------------------- |
| `astimezone(zona)`     | Puede cambiar  | Se conserva                     |
| `replace(tzinfo=zona)` | Se conserva    | Puede cambiar la interpretación |

## Reglas importantes

- Usa `astimezone()` para convertir fechas _aware_ entre zonas.
    
- Usa `replace(tzinfo=...)` para asignar una zona a una hora _naive_ cuyo significado local conoces.
    
- No uses `replace(tzinfo=UTC)` para convertir una hora local a UTC: asigna UTC sin ajustar la hora.
    
- Valida las horas ambiguas o inexistentes cuando trabajes con cambios estacionales.
    
- Recuerda que `astimezone()` sin argumentos convierte a la zona local del sistema.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html#datetime.datetime.astimezone) `[datetime.astimezone()](https://docs.python.org/3.14/library/datetime.html#datetime.datetime.astimezone)`.