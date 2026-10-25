# Funciones de agregación — `sum()`, `mean()`, `median()`, `min()`, `max()` y `count()`

Resume columnas mediante cálculos estadísticos para analizar cantidades, promedios, extremos y registros disponibles.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [101, 102, 103, 104],
    "peso": [10, 20, 30, None],
})

# Agregaciones sobre una columna
total = df["peso"].sum()
promedio = df["peso"].mean()
mediana = df["peso"].median()
minimo = df["peso"].min()
maximo = df["peso"].max()
cantidad = df["peso"].count()
```

## Funciones esenciales

- `sum()`: suma los valores.
    
- `mean()`: calcula la media aritmética.
    
- `median()`: obtiene el valor central.
    
- `min()` / `max()`: encuentran los valores mínimo y máximo.
    
- `count()`: cuenta los valores no ausentes.
    

## Patrones útiles

```python
# Agregar todas las columnas compatibles
resumen = df.agg({
    "pedido": "count",
    "peso": ["sum", "mean", "min", "max"],
})

# Agregar varias estadísticas a una columna
estadisticas = df["peso"].agg(["count", "sum", "mean", "median"])

# Contar filas, incluidos los valores ausentes en otras columnas
filas = len(df)

# Contar valores ausentes
ausentes = df["peso"].isna().sum()
```

## Reglas 80/20

- Aplica las funciones directamente a una `Series`: `df["peso"].mean()`.
    
- Por defecto, estas agregaciones numéricas suelen omitir valores ausentes.
    
- `count()` cuenta valores presentes; `len(df)` cuenta filas.
    
- Usa `agg()` para obtener varias estadísticas en una sola operación.
    
- Para resumir por categorías, combina estas funciones con `groupby()`.
    

**Versión de referencia:** Pandas 3.1.0.