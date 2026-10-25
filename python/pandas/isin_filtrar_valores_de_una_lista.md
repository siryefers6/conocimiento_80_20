# `isin()` — filtrar valores de una lista en Pandas

Utiliza `isin()` para seleccionar las filas cuyos valores pertenecen a una colección de elementos permitidos. Devuelve una máscara booleana que puedes aplicar al `DataFrame`.

## 1. Filtrar valores específicos

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [101, 102, 103, 104],
    "zona": ["Norte", "Sur", "Centro", "Norte"],
})

filtrado = df[df["zona"].isin(["Norte", "Sur"])]
```

## 2. Excluir valores específicos

Combina `isin()` con el operador `~`.

```python
filtrado = df[~df["zona"].isin(["Norte", "Sur"])]
```

## 3. Filtrar por varias columnas

```python
filtrado = df[
    df["zona"].isin(["Norte", "Centro"])
    & df["pedido"].isin([101, 103])
]
```

## 4. Obtener la máscara booleana

```python
mascara = df["zona"].isin(["Norte", "Sur"])

print(mascara)
```

Usa la máscara para filtrar, contar o combinar condiciones con otros criterios.

## Regla 80/20

Domina estos patrones:

- `df[df["columna"].isin(lista)]`: incluir valores.
    
- `df[~df["columna"].isin(lista)]`: excluir valores.
    
- `df["columna"].isin(lista).sum()`: contar coincidencias.
    

**Nota:** `isin()` compara valores exactos; distingue mayúsculas, minúsculas y tipos de datos.

**Versión de referencia:** Pandas 3.1.0.