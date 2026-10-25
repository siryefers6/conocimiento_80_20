# DataFrame — Pandas

Un `DataFrame` es una estructura bidimensional que organiza datos en filas y columnas, identificadas mediante índices y nombres.

## Crear un DataFrame

```python
import pandas as pd

df = pd.DataFrame({
    "nombre": ["Ana", "Luis", "Pedro"],
    "edad": [25, 30, 28],
})

print(df)
```

## Operaciones esenciales

```python
df["nombre"]       # Seleccionar una columna
df.loc[0]          # Seleccionar por etiqueta de índice
df.iloc[0]         # Seleccionar por posición
df.shape           # Número de filas y columnas
df.columns         # Nombres de las columnas
df.index           # Etiquetas de las filas
df.dtypes          # Tipos de datos por columna
df.head()          # Primeras cinco filas
```

## Regla 80/20

Domina la creación de tablas, la selección de columnas, el acceso a filas y la inspección de datos. Estas operaciones constituyen la base del análisis con Pandas.

**Versión:** Pandas 3.0.x (2026).