# Accesor `.dt` — extraer componentes temporales

**Pandas 3.1.0**

Usa `.dt` para acceder a los componentes y propiedades de una columna con fechas (`datetime64`). Permite extraer información temporal, filtrar registros y crear agrupaciones por períodos.

## Componentes esenciales

```python
import pandas as pd

df = pd.DataFrame({
    "fecha": pd.to_datetime([
        "2026-10-24 14:35:42",
        "2026-10-25 09:15:00"
    ])
})

df["anio"] = df["fecha"].dt.year
df["mes"] = df["fecha"].dt.month
df["dia"] = df["fecha"].dt.day
df["hora"] = df["fecha"].dt.hour
df["minuto"] = df["fecha"].dt.minute
df["dia_semana"] = df["fecha"].dt.dayofweek
```

`dayofweek` devuelve `0` para lunes y `6` para domingo.

## Propiedades más utilizadas

| Propiedad          | Resultado                  |
| ------------------ | -------------------------- |
| `.dt.year`         | Año                        |
| `.dt.month`        | Mes, del 1 al 12           |
| `.dt.day`          | Día del mes                |
| `.dt.hour`         | Hora, de 0 a 23            |
| `.dt.minute`       | Minutos                    |
| `.dt.second`       | Segundos                   |
| `.dt.dayofweek`    | Día de la semana, de 0 a 6 |
| `.dt.day_name()`   | Nombre del día             |
| `.dt.month_name()` | Nombre del mes             |
| `.dt.date`         | Fecha sin hora             |
| `.dt.time`         | Hora sin fecha             |
| `.dt.quarter`      | Trimestre, del 1 al 4      |

## Patrones prácticos

```python
# Filtrar registros de octubre
octubre = df[df["fecha"].dt.month == 10]

# Filtrar registros del año 2026
anuales = df[df["fecha"].dt.year == 2026]

# Agrupar por mes
resumen = df.groupby(df["fecha"].dt.month).size()

# Obtener el inicio del mes
df["inicio_mes"] = df["fecha"].dt.to_period("M")
```

## Regla práctica

Convierte primero la columna con `pd.to_datetime()` y después utiliza `.dt`. No confundas `.dt.month` —que devuelve un número— con `.dt.month_name()`, que devuelve el nombre del mes.