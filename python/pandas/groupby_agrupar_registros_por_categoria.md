# `groupby()` — agrupar registros por categoría

Agrupa filas según una o varias columnas para calcular totales, promedios, cantidades y otras estadísticas por grupo.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "zona": ["Norte", "Sur", "Norte", "Sur", "Norte"],
    "peso": [10, 20, 15, 25, 30],
    "pedido": [101, 102, 103, 104, 105],
})

# Peso total por zona
totales = df.groupby("zona")["peso"].sum()

# Promedio por zona
promedios = df.groupby("zona")["peso"].mean()

# Cantidad de registros por zona
cantidades = df.groupby("zona").size()
```

## Patrones esenciales

```python
# Varias estadísticas sobre una columna
resumen = df.groupby("zona")["peso"].agg(
    ["sum", "mean", "min", "max", "count"]
)

# Agrupar por varias columnas
resumen = df.groupby(
    ["zona", "pedido"]
)["peso"].sum()

# Obtener un DataFrame con la categoría como columna
resumen = (
    df.groupby("zona", as_index=False)
      .agg(
          peso_total=("peso", "sum"),
          peso_promedio=("peso", "mean"),
          cantidad=("pedido", "count"),
      )
)
```

## Reglas 80/20

- `groupby("columna")`: define los grupos.
    
- `sum()`, `mean()`, `min()`, `max()`: calculan estadísticas por grupo.
    
- `size()`: cuenta filas, incluso si algunas columnas contienen valores ausentes.
    
- `count()`: cuenta valores no ausentes de una columna.
    
- `agg()`: combina varias agregaciones en una operación.
    
- `as_index=False`: conserva las columnas agrupadoras como columnas normales.
    

**Prioriza:** `groupby()` combinado con `agg()` para construir informes por zona, supervisor, estado o categoría.

**Versión de referencia:** Pandas 3.1.0.