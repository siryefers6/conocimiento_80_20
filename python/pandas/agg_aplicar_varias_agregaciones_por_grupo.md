# `agg()` — aplicar varias agregaciones por grupo

Usa `agg()` para calcular varias estadísticas sobre una o más columnas, especialmente después de `groupby()`. Es esencial para generar informes resumidos.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "zona": ["Norte", "Sur", "Norte", "Sur"],
    "peso": [10, 20, 30, 40],
    "pedido": [101, 102, 103, 104],
})

# Varias agregaciones sobre una columna
resumen = df.groupby("zona")["peso"].agg(
    ["sum", "mean", "min", "max", "count"]
)
```

## Nombrar las columnas resultantes

```python
resumen = df.groupby("zona", as_index=False).agg(
    peso_total=("peso", "sum"),
    peso_promedio=("peso", "mean"),
    peso_maximo=("peso", "max"),
    cantidad_pedidos=("pedido", "count"),
)
```

## Agregaciones por columna

```python
resumen = df.agg({
    "peso": ["sum", "mean"],
    "pedido": ["min", "max", "count"],
})
```

## Reglas 80/20

- `agg(["sum", "mean"])`: aplica varias funciones a una columna.
    
- `agg(nombre=("columna", "función"))`: define nombres claros para los resultados.
    
- `groupby(...).agg(...)`: resume datos por categoría.
    
- `as_index=False`: mantiene las categorías como columnas.
    
- `count` cuenta valores no ausentes; `size` cuenta todas las filas del grupo.
    

**Prioriza:** la agregación con nombres explícitos para crear tablas listas para informes y exportaciones.

**Versión de referencia:** Pandas 3.1.0.