# Ordenamiento — `sort_values()` y `sort_index()`

Ordena registros por los valores de una o varias columnas o reorganiza filas según las etiquetas del índice.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [103, 101, 102],
    "peso": [30, 10, 20],
})

# Ordenar por una columna ascendente
por_peso = df.sort_values("peso")

# Ordenar de mayor a menor
por_peso = df.sort_values("peso", ascending=False)

# Ordenar por varias columnas
ordenado = df.sort_values(
    ["peso", "pedido"],
    ascending=[False, True],
)

# Ordenar por las etiquetas del índice
por_indice = df.sort_index()
```

## Parámetros esenciales

- `by`: columna o lista de columnas por las que ordenar.
    
- `ascending=True`: orden ascendente; usa `False` para descendente.
    
- `na_position="last"`: coloca los valores ausentes al final; también admite `"first"`.
    
- `ignore_index=True`: genera un índice nuevo consecutivo después de ordenar.
    

## Patrones útiles

```python
# Ordenar y reiniciar el índice
df = df.sort_values("peso", ignore_index=True)

# Ordenar por varias columnas y direcciones
df = df.sort_values(
    ["peso", "pedido"],
    ascending=[False, True],
)

# Ordenar el índice de forma descendente
df = df.sort_index(ascending=False)
```

## Reglas 80/20

- Usa `sort_values()` para ordenar por datos de negocio.
    
- Usa `sort_index()` para ordenar por etiquetas del índice.
    
- Ambos métodos devuelven un `DataFrame` ordenado sin modificar el original por defecto.
    
- No confundas ordenar filas con ordenar columnas; para ordenar columnas por nombre, utiliza `df.sort_index(axis=1)`.
    

**Versión de referencia:** Pandas 3.1.0.