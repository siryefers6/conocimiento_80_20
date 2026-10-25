# `pd.to_datetime()` — convertir valores en fechas

**Pandas 3.1.0**

Usa `pd.to_datetime()` para convertir cadenas, columnas o valores en objetos de fecha y hora compatibles con Pandas. Es fundamental para ordenar históricos, calcular duraciones y filtrar períodos.

## Conversión básica

```python
import pandas as pd

df = pd.DataFrame({
    "fecha": ["2026-10-20", "2026-10-21", "2026-10-22"]
})

df["fecha"] = pd.to_datetime(df["fecha"])

print(df.dtypes)
```

El tipo resultante normalmente es `datetime64[ns]`.

## Parámetros esenciales

- `format`: especifica el formato de entrada.
    
- `errors="coerce"`: convierte valores inválidos en `NaT`.
    
- `dayfirst=True`: prioriza el día en formatos ambiguos.
    
- `utc=True`: convierte los valores a fechas con zona horaria UTC.
    
- `format="mixed"`: infiere el formato de cada valor individualmente.
    

## Patrones de uso

```python
# Formato explícito: día/mes/año
df["fecha"] = pd.to_datetime(
    df["fecha"],
    format="%d/%m/%Y"
)

# Convertir valores inválidos en NaT
df["fecha"] = pd.to_datetime(
    df["fecha"],
    errors="coerce"
)

# Identificar fechas inválidas
invalidas = df[df["fecha"].isna()]

# Convertir fechas con hora
df["fecha_hora"] = pd.to_datetime(
    df["fecha_hora"],
    format="%Y-%m-%d %H:%M:%S"
)
```

## Códigos de formato frecuentes

| Código | Significado           | Ejemplo |
| ------ | --------------------- | ------- |
| `%Y`   | Año de cuatro dígitos | `2026`  |
| `%m`   | Mes                   | `10`    |
| `%d`   | Día                   | `24`    |
| `%H`   | Hora de 24 horas      | `16`    |
| `%M`   | Minutos               | `35`    |
| `%S`   | Segundos              | `42`    |

## Regla práctica

- Prefiere `format` explícito cuando conozcas el formato de origen.
    
- Usa `errors="coerce"` para limpiar datos y revisar después los `NaT`.
    
- No confundas `NaT` con una fecha válida: representa un valor temporal ausente o inválido.
    
- Una vez convertida la columna, utiliza `.dt` para extraer el año, mes, día o día de la semana.