# `loc[]` — selección por etiquetas y condiciones en Pandas

Utiliza `loc[]` para seleccionar filas por etiquetas de índice, columnas por nombre y registros que cumplan condiciones booleanas.

## 1. Seleccionar filas y columnas

```python
import pandas as pd

df = pd.DataFrame(
    {
        "nombre": ["Ana", "Luis", "Pedro"],
        "edad": [25, 30, 28],
    },
    index=["A", "B", "C"],
)

df.loc["A"]                  # Fila con índice A
df.loc["A", "nombre"]         # Valor: Ana
df.loc["A", ["nombre", "edad"]]
df.loc[:, "nombre"]           # Todas las filas de nombre
df.loc["A":"B", ["nombre"]]   # Rango inclusivo
```

## 2. Filtrar mediante condiciones

```python
df.loc[df["edad"] >= 28]
```

Combina condiciones con operadores booleanos:

```python
df.loc[
    (df["edad"] >= 25) & (df["edad"] < 30)
]
```

- `&`: AND lógico.
    
- `|`: OR lógico.
    
- `~`: negación lógica.
    

Encierra cada condición entre paréntesis.

## 3. Seleccionar y modificar datos

```python
df.loc[df["edad"] >= 30, "nombre"] = "Adulto"
```

Modifica directamente las filas y columnas seleccionadas, evitando asignaciones encadenadas.

## Regla 80/20

Domina estas tres formas:

- `df.loc[fila, columna]`: seleccionar por etiquetas.
    
- `df.loc[condicion]`: filtrar filas.
    
- `df.loc[condicion, columna] = valor`: modificar datos filtrados.
    

**Diferencia clave:** `loc[]` utiliza etiquetas; `iloc[]` utiliza posiciones enteras.

**Versión de referencia:** Pandas 3.1.0.