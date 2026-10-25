# `merge()` — unir DataFrames mediante claves

**Pandas 3.1.0**

Usa `pd.merge()` para combinar tablas mediante columnas comunes o claves específicas. Equivale a los `JOIN` de SQL.

## Tipos de unión esenciales

```python
import pandas as pd

pedidos = pd.DataFrame({
    "pedido_id": [1, 2, 3],
    "zona": ["Norte", "Sur", "Centro"]
})

despachos = pd.DataFrame({
    "pedido_id": [1, 2],
    "peso": [120, 85]
})
```

```python
# INNER JOIN: solo claves coincidentes
pd.merge(pedidos, despachos, on="pedido_id", how="inner")

# LEFT JOIN: conserva todos los pedidos
pd.merge(pedidos, despachos, on="pedido_id", how="left")

# RIGHT JOIN: conserva todos los despachos
pd.merge(pedidos, despachos, on="pedido_id", how="right")

# OUTER JOIN: conserva todas las claves de ambas tablas
pd.merge(pedidos, despachos, on="pedido_id", how="outer")
```

## Parámetros importantes

- `on`: columna clave presente en ambas tablas.
    
- `left_on` y `right_on`: claves con nombres diferentes.
    
- `how`: tipo de unión (`inner`, `left`, `right` u `outer`).
    
- `suffixes`: sufijos para columnas con nombres repetidos.
    
- `validate`: comprueba la cardinalidad esperada de la relación.
    
- `indicator=True`: añade `_merge` para identificar el origen de cada fila.
    

## Ejemplo práctico

```python
resultado = pd.merge(
    pedidos,
    despachos,
    on="pedido_id",
    how="left",
    validate="one_to_one",
    indicator=True
)

print(resultado)
```

El resultado conserva todos los pedidos. El pedido `3` tendrá `NaN` en `peso` y `_merge` indicará `left_only`.

## Reglas importantes

- Verifica que las claves tengan tipos compatibles antes de unir.
    
- Comprueba los valores nulos y duplicados de las claves.
    
- Si una clave aparece varias veces en ambas tablas, la unión puede multiplicar filas.
    
- Usa `validate="one_to_one"`, `"one_to_many"` o `"many_to_one"` para detectar relaciones inesperadas.
    
- Usa `indicator=True` para identificar registros sin correspondencia.
    

**Regla práctica:** utiliza `merge()` para relacionar tablas mediante claves; elige `how="left"` cuando debas conservar todos los registros de la tabla principal.