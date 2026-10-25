# `iloc[]` — selección por posiciones enteras en Pandas

Utiliza `iloc[]` para seleccionar filas y columnas mediante posiciones enteras, comenzando desde `0`. No depende de las etiquetas del índice.

## 1. Seleccionar filas y columnas

```python
import pandas as pd

df = pd.DataFrame({
    "nombre": ["Ana", "Luis", "Pedro"],
    "edad": [25, 30, 28],
    "ciudad": ["Santiago", "Valparaíso", "Concepción"],
})

df.iloc[0]        # Primera fila
df.iloc[0, 1]     # Primera fila, segunda columna: 25
df.iloc[:, 0]     # Todas las filas, primera columna
df.iloc[0:2, 0:2] # Primeras 2 filas y 2 columnas
df.iloc[-1]       # Última fila
```

## 2. Seleccionar posiciones específicas

```python
df.iloc[[0, 2]]       # Filas en posiciones 0 y 2
df.iloc[:, [0, 2]]    # Columnas en posiciones 0 y 2
df.iloc[0:3:2]        # Filas con paso de 2
```

Los rangos siguen la regla de Python: incluyen el inicio y excluyen el final.

## 3. Modificar valores

```python
df.iloc[0, 1] = 26
```

Modifica el valor ubicado en la primera fila y segunda columna.

## Regla 80/20

Domina estas formas:

- `df.iloc[fila]`: seleccionar una fila.
    
- `df.iloc[fila, columna]`: seleccionar una celda.
    
- `df.iloc[:, columna]`: seleccionar una columna.
    
- `df.iloc[inicio:fin]`: seleccionar un rango de filas.
    

**Diferencia clave:** `loc[]` selecciona por etiquetas; `iloc[]` selecciona por posiciones enteras.

**Versión de referencia:** Pandas 3.1.0.