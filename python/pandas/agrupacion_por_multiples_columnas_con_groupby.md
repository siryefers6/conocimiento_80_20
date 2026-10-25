# Agrupación por múltiples columnas con `groupby()`

**Pandas 3.1.0**

Usa `groupby()` con una lista de columnas para analizar combinaciones de categorías, como zona y estado, supervisor y ruta, o mes y producto.

## Agrupar por varias columnas

```python
import pandas as pd

df = pd.DataFrame({
    "zona": ["Norte", "Norte", "Sur", "Sur", "Norte"],
    "estado": ["Entregado", "Pendiente", "Entregado", "Entregado", "Entregado"],
    "peso": [100, 50, 80, 120, 70]
})

resultado = (
    df.groupby(["zona", "estado"])["peso"]
    .sum()
    .reset_index()
)

print(resultado)
```

Resultado:

```
    zona     estado  peso
0  Norte   Entregado   170
1  Norte   Pendiente    50
2    Sur   Entregado   200
```

## Patrones esenciales

```python
# Contar registros por combinación
df.groupby(["zona", "estado"]).size()

# Calcular varias métricas
df.groupby(["zona", "estado"]).agg(
    cantidad=("peso", "size"),
    peso_total=("peso", "sum"),
    peso_promedio=("peso", "mean")
)

# Ordenar por peso total descendente
(
    df.groupby(["zona", "estado"])["peso"]
    .sum()
    .sort_values(ascending=False)
)

# Mantener las categorías como columnas
df.groupby(["zona", "estado"], as_index=False)["peso"].sum()
```

## Conceptos clave

- `groupby(["columna1", "columna2"])`: crea grupos para cada combinación única.
    
- `.size()`: cuenta filas, incluso si contienen valores nulos en otras columnas.
    
- `.agg()`: calcula varias métricas por grupo.
    
- `.reset_index()`: convierte los niveles del índice en columnas.
    
- `as_index=False`: devuelve las claves de agrupación como columnas.
    

**Regla práctica:** agrupa por varias columnas cuando necesites desglosar una métrica por combinaciones de categorías. Usa `crosstab()` para tablas de frecuencias y `pivot_table()` para tablas dinámicas.