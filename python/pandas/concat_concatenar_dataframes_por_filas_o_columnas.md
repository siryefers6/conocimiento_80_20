# `concat()` — concatenar DataFrames por filas o columnas

**Pandas 3.1.0**

Usa `pd.concat()` para unir varios DataFrames a lo largo de un eje. A diferencia de `merge()`, no necesita claves de unión.

## Concatenar por filas

Apila registros verticalmente. Las columnas deben corresponder a los mismos campos.

```python
import pandas as pd

enero = pd.DataFrame({
    "pedido": [101, 102],
    "peso": [100, 150]
})

febrero = pd.DataFrame({
    "pedido": [103, 104],
    "peso": [120, 180]
})

resultado = pd.concat([enero, febrero], ignore_index=True)
```

- `axis=0`: concatena por filas; es el valor predeterminado.
    
- `ignore_index=True`: crea un índice nuevo y consecutivo.
    

## Concatenar por columnas

Combina columnas alineando los DataFrames por sus índices.

```python
datos_a = pd.DataFrame({"pedido": [101, 102]})
datos_b = pd.DataFrame({"peso": [100, 150]})

resultado = pd.concat([datos_a, datos_b], axis=1)
```

- `axis=1`: concatena horizontalmente.
    
- Los índices determinan qué filas quedan alineadas.
    

## Parámetros esenciales

```python
# Conservar solo las columnas compartidas
pd.concat([enero, febrero], join="inner", ignore_index=True)

# Mantener todas las columnas; las ausentes quedan como NaN
pd.concat([enero, febrero], join="outer", ignore_index=True)

# Identificar el origen de cada bloque
pd.concat(
    [enero, febrero],
    keys=["enero", "febrero"],
    names=["mes"]
)
```

- `join="outer"`: conserva la unión de las columnas o índices del otro eje.
    
- `join="inner"`: conserva su intersección.
    
- `keys`: identifica el origen de cada bloque mediante un índice jerárquico.
    

## Reglas prácticas

- Usa `concat()` para apilar archivos o lotes con estructura similar.
    
- Usa `axis=1` para colocar columnas lado a lado alineadas por índice.
    
- Usa `merge()` cuando necesites relacionar tablas mediante claves.
    
- Evita concatenar repetidamente dentro de un bucle; acumula los DataFrames en una lista y concatena una sola vez.