# `join()` — combinar DataFrames mediante índices

**Pandas 3.1.0**

Usa `DataFrame.join()` para combinar columnas de dos DataFrames utilizando sus índices. Es útil cuando ambas tablas ya tienen un índice que identifica sus registros.

## Unión básica

```python
import pandas as pd

pedidos = pd.DataFrame(
    {"zona": ["Norte", "Sur", "Centro"]},
    index=[101, 102, 103]
)

despachos = pd.DataFrame(
    {"peso": [120, 85]},
    index=[101, 102]
)

resultado = pedidos.join(despachos, how="left")
print(resultado)
```

Resultado:

```
        zona   peso
101    Norte  120.0
102      Sur   85.0
103   Centro    NaN
```

## Tipos de unión

```python
# Conserva todos los índices del DataFrame izquierdo
pedidos.join(despachos, how="left")

# Conserva solo los índices coincidentes
pedidos.join(despachos, how="inner")

# Conserva todos los índices de ambos DataFrames
pedidos.join(despachos, how="outer")
```

## Combinar por una columna

Usa `on` cuando la clave del DataFrame izquierdo está en una columna y coincide con el índice del derecho.

```python
pedidos = pd.DataFrame({
    "pedido_id": [101, 102, 103],
    "zona": ["Norte", "Sur", "Centro"]
})

despachos = pd.DataFrame(
    {"peso": [120, 85]},
    index=[101, 102]
)

resultado = pedidos.join(
    despachos,
    on="pedido_id",
    how="left"
)
```

## Parámetros esenciales

- `on`: columna del DataFrame izquierdo que se compara con el índice del derecho.
    
- `how`: tipo de unión (`left`, `inner`, `right` u `outer`).
    
- `lsuffix` y `rsuffix`: sufijos para resolver nombres de columnas duplicados.
    
- `validate`: comprueba la cardinalidad esperada de la relación.
    

**Regla práctica:** utiliza `join()` para combinar datos por índices y `merge()` cuando las claves estén en columnas de una o ambas tablas.