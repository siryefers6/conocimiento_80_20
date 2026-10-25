# Tipos de unión en Pandas — `inner`, `left`, `right` y `outer`

**Pandas 3.1.0**

El parámetro `how` de `merge()` determina qué registros conservar al combinar dos DataFrames mediante una clave.

## Ejemplo base

```python
import pandas as pd

pedidos = pd.DataFrame({
    "id": [1, 2, 3],
    "zona": ["Norte", "Sur", "Centro"]
})

despachos = pd.DataFrame({
    "id": [2, 3, 4],
    "peso": [100, 150, 200]
})
```

## Tipos de unión

```python
# Solo registros con claves coincidentes
pd.merge(pedidos, despachos, on="id", how="inner")

# Todos los registros de pedidos
pd.merge(pedidos, despachos, on="id", how="left")

# Todos los registros de despachos
pd.merge(pedidos, despachos, on="id", how="right")

# Todos los registros de ambas tablas
pd.merge(pedidos, despachos, on="id", how="outer")
```

| Tipo    | Claves conservadas        | Resultado       |
| ------- | ------------------------- | --------------- |
| `inner` | Coincidentes              | IDs 2 y 3       |
| `left`  | Todas las de `pedidos`    | IDs 1, 2 y 3    |
| `right` | Todas las de `despachos`  | IDs 2, 3 y 4    |
| `outer` | Todas las de ambas tablas | IDs 1, 2, 3 y 4 |

Los campos sin correspondencia reciben `NaN`.

## Regla práctica

- `inner`: analiza únicamente coincidencias.
    
- `left`: conserva la tabla principal, aunque falten datos relacionados.
    
- `right`: conserva la tabla derecha.
    
- `outer`: identifica registros presentes en cualquiera de las tablas.
    

**Prioriza** `**left**` cuando una tabla representa el conjunto principal que no debes perder. Verifica también si las claves están duplicadas: una unión puede generar varias filas por cada coincidencia.