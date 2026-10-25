# `crosstab()` — construir tablas de frecuencias cruzadas

**Pandas 3.1.0**

Usa `pd.crosstab()` para contar la frecuencia de combinaciones entre categorías. Ideal para analizar relaciones entre zonas, estados, meses o tipos.

## Frecuencia cruzada básica

```python
import pandas as pd

df = pd.DataFrame({
    "zona": ["Norte", "Sur", "Norte", "Sur", "Norte"],
    "estado": ["Entregado", "Pendiente", "Pendiente", "Entregado", "Entregado"]
})

tabla = pd.crosstab(df["zona"], df["estado"])
print(tabla)
```

Resultado:

```
estado  Entregado  Pendiente
zona
Norte           2          1
Sur             1          1
```

## Parámetros esenciales

- `index`: categorías que forman las filas.
    
- `columns`: categorías que forman las columnas.
    
- `values` y `aggfunc`: permiten agregar valores numéricos en vez de contar registros.
    
- `normalize`: calcula proporciones en lugar de frecuencias absolutas.
    
- `margins=True`: agrega totales por fila y columna.
    

## Patrones de uso

```python
# Porcentajes del total
pd.crosstab(df["zona"], df["estado"], normalize="all")

# Porcentajes por fila
pd.crosstab(df["zona"], df["estado"], normalize="index")

# Porcentajes por columna
pd.crosstab(df["zona"], df["estado"], normalize="columns")

# Totales por fila y columna
pd.crosstab(df["zona"], df["estado"], margins=True)

# Agregar una columna numérica por categoría
pd.crosstab(
    df["zona"],
    df["estado"],
    values=[10, 20, 15, 30, 25],
    aggfunc="sum"
)
```

## Regla práctica

- Usa `crosstab()` para **contar o comparar categorías**.
    
- Usa `normalize` para analizar porcentajes.
    
- Usa `margins=True` para obtener totales.
    
- Usa `pivot_table()` cuando necesites una tabla dinámica con mayor control sobre los valores y sus agregaciones.