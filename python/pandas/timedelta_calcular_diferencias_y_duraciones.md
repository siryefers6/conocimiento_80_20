# `Timedelta` — calcular diferencias y duraciones

**Pandas 3.1.0**

Usa `Timedelta` para representar duraciones y calcular diferencias entre fechas u horas. Es útil para medir tiempos de espera, procesamiento y cumplimiento de plazos.

## Calcular diferencias entre fechas

```python
import pandas as pd

df = pd.DataFrame({
    "inicio": pd.to_datetime(["2026-10-24 08:00", "2026-10-24 09:30"]),
    "fin": pd.to_datetime(["2026-10-24 10:15", "2026-10-24 11:00"])
})

df["duracion"] = df["fin"] - df["inicio"]

print(df["duracion"])
```

Resultado:

```
0   0 days 02:15:00
1   0 days 01:30:00
```

La resta de dos fechas produce una serie de tipo `timedelta64[ns]`.

## Convertir duraciones a unidades

```python
# Duración en segundos
df["segundos"] = df["duracion"].dt.total_seconds()

# Duración en minutos
df["minutos"] = df["duracion"].dt.total_seconds() / 60

# Duración en horas
df["horas"] = df["duracion"].dt.total_seconds() / 3600
```

Usa `total_seconds()` para obtener la duración completa, incluso cuando supera un día.

## Crear duraciones con `Timedelta`

```python
# Añadir 30 minutos a una fecha
df["limite"] = df["inicio"] + pd.Timedelta(minutes=30)

# Restar 2 horas
df["anterior"] = df["inicio"] - pd.Timedelta(hours=2)

# Comparar con una duración máxima
df["supera_limite"] = df["duracion"] > pd.Timedelta(hours=2)
```

Unidades frecuentes: `days`, `hours`, `minutes`, `seconds` y `milliseconds`.

## Calcular tiempo entre registros

```python
df = df.sort_values("inicio")

df["espera"] = df["inicio"].diff()
```

`diff()` calcula la diferencia respecto al registro anterior. Ordena primero por fecha para obtener resultados cronológicamente correctos.

## Regla práctica

- Resta fechas para obtener duraciones.
    
- Usa `pd.Timedelta()` para sumar o comparar intervalos.
    
- Usa `.dt.total_seconds()` para convertir duraciones completas a segundos.
    
- Distingue `Timedelta` —una duración— de `Timestamp` —un instante temporal—.