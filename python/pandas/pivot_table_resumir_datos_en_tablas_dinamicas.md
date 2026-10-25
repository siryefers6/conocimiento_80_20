# `pivot_table()` — resumir datos en tablas dinámicas

Usa `pivot_table()` para resumir datos por filas y columnas, aplicando agregaciones como sumas, promedios o conteos. Es útil para informes comparativos.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "zona": ["Norte", "Sur", "Norte", "Sur", "Norte"],
    "mes": ["Enero", "Enero", "Febrero", "Febrero", "Enero"],
    "peso": [10, 20, 15, 25, 30],
})

# Peso total por zona y mes
tabla = df.pivot_table(
    index="zona",
    columns="mes",
    values="peso",
    aggfunc="sum",
    fill_value=0,
)
```

## Parámetros esenciales

- `index`: categorías que forman las filas.
    
- `columns`: categorías que se convierten en columnas.
    
- `values`: columna sobre la que se calcula la agregación.
    
- `aggfunc`: función de agregación; por defecto, `mean`.
    
- `fill_value`: valor que reemplaza resultados ausentes en la tabla.
    

## Patrones útiles

```python
# Promedio de peso por zona y mes
promedios = df.pivot_table(
    index="zona",
    columns="mes",
    values="peso",
    aggfunc="mean",
)

# Varias estadísticas por grupo
resumen = df.pivot_table(
    index="zona",
    values="peso",
    aggfunc=["sum", "mean", "count"],
)

# Contar registros por zona y mes
conteo = df.pivot_table(
    index="zona",
    columns="mes",
    values="peso",
    aggfunc="count",
    fill_value=0,
)
```

## Reglas 80/20

- Usa `index` y `columns` para definir la estructura del informe.
    
- Usa `values` para seleccionar los datos que deseas resumir.
    
- Usa `aggfunc="sum"` para totales y `aggfunc="mean"` para promedios.
    
- Usa `fill_value=0` cuando los grupos sin resultados deban mostrarse como cero.
    
- Para resúmenes sin estructura de filas y columnas cruzadas, prioriza `groupby().agg()`.
    

**Versión de referencia:** Pandas 3.1.0.