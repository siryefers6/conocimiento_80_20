# Index — Pandas

Un `Index` almacena las etiquetas que identifican filas o columnas. Permite acceder a los datos y alinearlos automáticamente durante operaciones entre estructuras de Pandas.

## Crear y consultar un Index

```python
import pandas as pd

indice = pd.Index(["A", "B", "C"])

print(indice[0])          # A
print("B" in indice)      # True
print(indice.is_unique)   # True
```

## Operaciones esenciales

```python
df.index                 # Etiquetas de las filas
df.columns               # Etiquetas de las columnas
df.set_index("nombre")   # Usar una columna como índice
df.reset_index()         # Restaurar el índice predeterminado
df.index.is_unique       # Comprobar etiquetas únicas
df.reindex(["A", "C"])   # Reordenar o seleccionar etiquetas
```

## Alineación automática

Pandas alinea los datos mediante sus etiquetas, no únicamente por posición.

```python
a = pd.Series([10, 20], index=["A", "B"])
b = pd.Series([30, 40], index=["B", "C"])

print(a + b)
```

Resultado:

```
A     NaN
B    50.0
C     NaN
dtype: float64
```

## Regla 80/20

Domina `index`, `columns`, `set_index()`, `reset_index()` y la alineación automática. Recuerda que las etiquetas duplicadas están permitidas, aunque pueden complicar ciertas operaciones.

**Versión de referencia:** Pandas 3.1.0 (indicada por el usuario).