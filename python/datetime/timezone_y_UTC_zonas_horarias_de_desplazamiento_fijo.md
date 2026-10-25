# `timezone` y `UTC` — Zonas horarias de desplazamiento fijo

**Python 3.14 · Biblioteca estándar**

Utiliza `timezone` para representar UTC y zonas horarias con desplazamientos fijos respecto de UTC. Usa `UTC` como constante para representar el tiempo universal coordinado.

## Crear fechas con zona horaria

```python
from datetime import UTC, datetime, timedelta, timezone

ahora_utc = datetime.now(UTC)

zona_chile_fija = timezone(timedelta(hours=-3))
ahora_chile = datetime.now(zona_chile_fija)

print(ahora_utc)
print(ahora_chile)
```

## Crear un desplazamiento fijo

```python
from datetime import timedelta, timezone

utc_menos_3 = timezone(timedelta(hours=-3))
utc_mas_5_30 = timezone(timedelta(hours=5, minutes=30))
```

El desplazamiento debe estar entre `-24` y `+24` horas, sin incluir esos límites, y expresarse con precisión de minutos.

## Convertir entre zonas

```python
from datetime import UTC, datetime, timedelta, timezone

fecha_utc = datetime(2026, 10, 24, 12, 0, tzinfo=UTC)
zona_fija = timezone(timedelta(hours=-3))

print(fecha_utc.astimezone(zona_fija))
# 2026-10-24 09:00:00-03:00
```

`astimezone()` conserva el instante y ajusta su representación a la zona de destino.

## Reglas importantes

- Usa `UTC` para registros temporales normalizados.
    
- Usa `timezone(timedelta(...))` cuando necesites un desplazamiento fijo.
    
- No confundas un desplazamiento fijo con una zona geográfica que aplica cambios horarios.
    
- Usa `zoneinfo.ZoneInfo`, por ejemplo `ZoneInfo("America/Santiago")`, cuando necesites las reglas históricas y estacionales de una región.
    
- No elimines `tzinfo` si necesitas conservar la referencia al instante original.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html#timezone-objects) `[timezone](https://docs.python.org/3.14/library/datetime.html#timezone-objects)`.