# CSV — `read_csv()` y `to_csv()` en Pandas

Lee archivos CSV con `pd.read_csv()` y exporta datos con `DataFrame.to_csv()`. Controla delimitadores, codificación, columnas y valores ausentes.

## 1. Leer un archivo CSV

```python
import pandas as pd

df = pd.read_csv("datos.csv")
```

## 2. Controlar la lectura

```python
df = pd.read_csv(
    "datos.csv",
    sep=";",                 # Separador
    encoding="utf-8",        # Codificación
    decimal=",",             # Separador decimal
    na_values=["N/A", "-"],  # Valores considerados ausentes
    usecols=["nombre", "precio"],
)
```

Usa `sep=";"` para archivos separados por punto y coma, habituales en exportaciones regionales.

## 3. Exportar un archivo CSV

```python
df.to_csv(
    "resultado.csv",
    index=False,
    encoding="utf-8",
)
```

- `index=False`: evita exportar el índice como columna adicional.
    
- `sep=";"`: define el delimitador.
    
- `columns=["nombre", "precio"]`: selecciona las columnas.
    
- `na_rep=""`: representa los valores ausentes con cadenas vacías.
    

## 4. Leer archivos grandes por bloques

```python
for bloque in pd.read_csv("datos.csv", chunksize=10000):
    print(bloque.shape)
```

Procesa archivos grandes sin cargar todo su contenido simultáneamente en memoria.

## Regla 80/20

Domina `read_csv()`, `to_csv()`, `sep`, `encoding`, `usecols`, `index=False` y `chunksize`. Inspecciona siempre el resultado con `head()`, `shape` e `info()`.

**Versión de referencia:** Pandas 3.1.0.