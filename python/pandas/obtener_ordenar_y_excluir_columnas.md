# Selección de columnas — obtener, ordenar y excluir columnas en Pandas

Selecciona columnas por nombre para extraer, reorganizar o excluir datos de un `DataFrame`.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [101, 102],
    "zona": ["Norte", "Sur"],
    "peso": [15, 25],
})

# Obtener una columna como Series
zonas = df["zona"]

# Obtener varias columnas como DataFrame
resultado = df[["pedido", "peso"]]

# Ordenar columnas
resultado = df[["peso", "pedido", "zona"]]

# Excluir columnas
resultado = df.drop(columns=["zona"])

# Seleccionar columnas por etiqueta
resultado = df.loc[:, ["pedido", "zona"]]
```

## Reglas 80/20

- `df["columna"]`: devuelve una `Series`.
    
- `df[["col1", "col2"]]`: devuelve un `DataFrame`.
    
- `df.drop(columns=[...])`: excluye columnas sin modificar el original por defecto.
    
- `df.loc[:, [...]]`: selecciona filas y columnas mediante etiquetas; `:` incluye todas las filas.
    
- El orden de las columnas seleccionadas coincide con el de la lista proporcionada.
    

**Prioriza:** `df[["col1", "col2"]]` para seleccionar y ordenar; `drop(columns=[...])` para excluir.

**Versión de referencia:** Pandas 3.1.0.