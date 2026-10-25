# `indicator=True` — identificar el origen de los registros tras una unión

**Pandas 3.1.0**

Usa `indicator=True` en `merge()` para identificar si cada registro aparece solo en la tabla izquierda, solo en la derecha o en ambas.

## Ejemplo básico

```python
import pandas as pd

pedidos = pd.DataFrame({
    "pedido_id": [101, 102, 103]
})

despachos = pd.DataFrame({
    "pedido_id": [102, 103, 104],
    "peso": [50, 80, 100]
})

resultado = pd.merge(
    pedidos,
    despachos,
    on="pedido_id",
    how="outer",
    indicator=True
)

print(resultado)
```

Resultado:

```
   pedido_id   peso      _merge
0        101    NaN   left_only
1        102   50.0        both
2        103   80.0        both
3        104  100.0  right_only
```

## Valores de `_merge`

| Valor        | Significado                                   |
| ------------ | --------------------------------------------- |
| `left_only`  | Registro presente solo en la tabla izquierda. |
| `right_only` | Registro presente solo en la tabla derecha.   |
| `both`       | Clave encontrada en ambas tablas.             |

## Patrones esenciales

```python
# Registros sin despacho
resultado[resultado["_merge"] == "left_only"]

# Despachos sin pedido correspondiente
resultado[resultado["_merge"] == "right_only"]

# Registros coincidentes
resultado[resultado["_merge"] == "both"]

# Contar registros por origen
resultado["_merge"].value_counts()
```

También puedes asignar un nombre personalizado a la columna:

```python
pd.merge(
    pedidos,
    despachos,
    on="pedido_id",
    how="outer",
    indicator="origen"
)
```

## Regla práctica

Combina `indicator=True` con `how="outer"` para auditar las coincidencias y detectar registros sin correspondencia en ninguna de las dos direcciones. Recuerda que identifica el origen de las filas resultantes, no garantiza que las claves sean únicas.