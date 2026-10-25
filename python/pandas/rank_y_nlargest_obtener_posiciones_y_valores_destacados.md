# `rank()` y `nlargest()` — obtener posiciones y valores destacados

Calcula la posición relativa de cada valor o selecciona los mayores valores de una columna sin ordenar todo el `DataFrame`.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [101, 102, 103, 104],
    "peso": [20, 50, 30, 50],
})

# Asignar posiciones según el peso
df["ranking"] = df["peso"].rank(
    ascending=False,
    method="dense",
)

# Obtener los 3 pesos más altos
mayores = df.nlargest(3, "peso")

# Obtener los 3 pedidos con mayor peso
mayores = df.nlargest(3, "peso")[["pedido", "peso"]]
```

## Diferencias esenciales

- `rank()`: asigna una posición a cada valor; por defecto, los empates reciben el rango promedio.
    
- `nlargest(n, columna)`: devuelve las `n` filas con los valores más altos de una columna.
    
- `nsmallest(n, columna)`: devuelve las `n` filas con los valores más bajos.
    

## Patrones útiles

```python
# Ranking sin saltos entre posiciones distintas
df["ranking"] = df["peso"].rank(
    ascending=False,
    method="dense",
)

# Ranking con saltos después de empates
df["ranking"] = df["peso"].rank(
    ascending=False,
    method="min",
)

# Desempatar seleccionando todas las filas empatadas en el límite
mayores = df.nlargest(3, "peso", keep="all")
```

## Reglas 80/20

- Usa `rank(ascending=False)` para clasificar de mayor a menor.
    
- Usa `method="dense"` para un ranking consecutivo sin saltos.
    
- Usa `nlargest()` para obtener rápidamente los registros destacados.
    
- `nlargest()` devuelve como máximo `n` filas por defecto; `keep="all"` puede devolver más si hay empates.
    
- Para ordenar el conjunto completo, utiliza `sort_values()`.
    

**Versión de referencia:** Pandas 3.1.0.