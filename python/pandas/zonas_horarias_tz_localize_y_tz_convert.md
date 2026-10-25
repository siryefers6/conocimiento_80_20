# Zonas horarias — `tz_localize()` y `tz_convert()`

**Pandas 3.1.0**

Usa `tz_localize()` para asignar una zona horaria a fechas sin zona y `tz_convert()` para convertir fechas que ya tienen zona horaria.

## 1. Asignar una zona: `tz_localize()`

Interpreta las fechas como pertenecientes a una zona horaria, sin cambiar la hora indicada.

```python
import pandas as pd

fechas = pd.to_datetime([
    "2026-10-24 09:00",
    "2026-10-24 15:30",
])

fechas = fechas.tz_localize("America/Santiago")
```

## 2. Convertir una zona: `tz_convert()`

Convierte las fechas al horario equivalente de otra zona.

```python
fechas_utc = fechas.tz_convert("UTC")
fechas_madrid = fechas.tz_convert("Europe/Madrid")
```

El instante real se conserva; cambia la hora mostrada.

## 3. Aplicarlo a una columna

```python
df = pd.DataFrame({
    "fecha": pd.to_datetime(["2026-10-24 09:00"])
})

df["fecha"] = (
    df["fecha"]
    .dt.tz_localize("America/Santiago")
    .dt.tz_convert("UTC")
)
```

## Reglas esenciales

- Usa nombres IANA, como `"America/Santiago"` o `"UTC"`.
    
- `tz_localize()` se aplica a fechas sin zona horaria.
    
- `tz_convert()` se aplica a fechas que ya tienen zona horaria.
    
- Para una columna, utiliza el accesor `.dt`.
    
- Los cambios de horario de verano pueden producir horas ambiguas o inexistentes; gestiona esos casos con `ambiguous` y `nonexistent` en `tz_localize()`.
    

**Prioriza:** almacenar instantes con zona horaria y convertirlos a la zona local solo al presentar los datos.