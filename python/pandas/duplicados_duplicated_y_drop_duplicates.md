# Duplicados — `duplicated()` y `drop_duplicates()`

Detecta y elimina registros duplicados para evitar contar datos repetidos en los análisis.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [101, 102, 101, 103],
    "zona": ["Norte", "Sur", "Norte", "Centro"],
})

# Detectar filas duplicadas
duplicados = df.duplicated()

# Filtrar solo las filas duplicadas
df_duplicados = df[df.duplicated()]

# Eliminar duplicados y conservar la primera aparición
df_limpio = df.drop_duplicates()
```

## Duplicados por columnas

```python
# Detectar pedidos repetidos, independientemente de otras columnas
df.duplicated(subset=["pedido"])

# Conservar la última aparición de cada pedido
df_limpio = df.drop_duplicates(
    subset=["pedido"],
    keep="last",
)

# Eliminar todas las filas cuyo pedido esté repetido
df_limpio = df.drop_duplicates(
    subset=["pedido"],
    keep=False,
)
```

## Reglas 80/20

- `duplicated()`: devuelve una máscara booleana; por defecto marca las apariciones posteriores.
    
- `drop_duplicates()`: elimina duplicados y conserva la primera aparición por defecto.
    
- `subset=[...]`: define qué columnas determinan si un registro está duplicado.
    
- `keep="first"`: conserva la primera aparición.
    
- `keep="last"`: conserva la última.
    
- `keep=False`: elimina todas las apariciones de los grupos duplicados.
    

**Precaución:** decide qué columnas identifican realmente un registro único antes de eliminar duplicados.

**Versión de referencia:** Pandas 3.1.0.