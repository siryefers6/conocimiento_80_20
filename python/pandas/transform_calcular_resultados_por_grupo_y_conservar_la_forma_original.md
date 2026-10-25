# `transform()` — calcular resultados por grupo y conservar la forma original

Usa `transform()` para calcular estadísticas por grupo y devolver un resultado con la misma cantidad de filas que el `DataFrame` original. Es útil para comparar cada registro con su grupo.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "zona": ["Norte", "Sur", "Norte", "Sur"],
    "peso": [10, 20, 30, 40],
})

# Promedio de peso de cada zona, repetido en sus filas
df["promedio_zona"] = (
    df.groupby("zona")["peso"].transform("mean")
)

# Diferencia respecto al promedio de la zona
df["diferencia"] = df["peso"] - df["promedio_zona"]
```

## Diferencias con `agg()`

- `agg()`: resume cada grupo y normalmente devuelve menos filas.
    
- `transform()`: devuelve un resultado alineado con las filas originales.
    
- `transform()` permite asignar directamente el resultado como una nueva columna.
    

## Patrones útiles

```python
# Normalizar el peso respecto al promedio de cada zona
df["peso_relativo"] = (
    df["peso"] / df.groupby("zona")["peso"].transform("mean")
)

# Obtener el máximo de cada grupo en todas sus filas
df["maximo_zona"] = (
    df.groupby("zona")["peso"].transform("max")
)

# Calcular la desviación respecto al promedio grupal
df["desviacion"] = (
    df["peso"] - df.groupby("zona")["peso"].transform("mean")
)
```

## Reglas 80/20

- Usa `groupby().transform("mean")` para comparar registros con la media de su grupo.
    
- Usa `transform("sum")`, `transform("min")` o `transform("max")` para distribuir estadísticas grupales entre las filas.
    
- Prioriza `transform()` cuando necesites conservar todas las filas.
    
- Evita recorrer grupos manualmente si una transformación vectorizada resuelve el cálculo.
    

**Versión de referencia:** Pandas 3.1.0.