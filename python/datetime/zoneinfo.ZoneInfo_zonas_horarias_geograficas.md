# `zoneinfo.ZoneInfo` — Zonas horarias geográficas

**Python 3.14 · Biblioteca estándar**

Utiliza `zoneinfo.ZoneInfo` para aplicar las reglas horarias de una región geográfica, incluidos los cambios estacionales y las reglas históricas definidas en la base de datos IANA.

## Crear una fecha con zona horaria

```python
from datetime import datetime
from zoneinfo import ZoneInfo

zona = ZoneInfo("America/Santiago")
ahora = datetime.now(zona)

print(ahora)
```

## Convertir entre zonas horarias

```python
from datetime import UTC, datetime
from zoneinfo import ZoneInfo

fecha_utc = datetime.now(UTC)
fecha_chile = fecha_utc.astimezone(ZoneInfo("America/Santiago"))

print(fecha_chile)
```

`astimezone()` conserva el instante y ajusta su representación a la zona de destino.

## Interpretar una hora local

```python
from datetime import datetime
from zoneinfo import ZoneInfo

fecha = datetime(2026, 10, 24, 9, 30)
fecha_chile = fecha.replace(tzinfo=ZoneInfo("America/Santiago"))

print(fecha_chile)
```

`replace(tzinfo=...)` asigna una zona horaria sin convertir la hora. Úsalo solo cuando los valores originales representen una hora local de esa región.

## Reglas importantes

- Usa identificadores IANA como `America/Santiago`, no abreviaturas como `CLT`.
    
- Prefiere `ZoneInfo` frente a `timezone(timedelta(...))` para zonas geográficas.
    
- Ten en cuenta que algunas horas locales pueden ser ambiguas o inexistentes durante cambios de horario.
    
- Si falta la base de datos de zonas horarias del sistema, instala `tzdata` cuando sea necesario.
    
- Usa objetos _aware_ para comparar instantes de distintas zonas.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/zoneinfo.html) `[zoneinfo](https://docs.python.org/3.14/library/zoneinfo.html)`.