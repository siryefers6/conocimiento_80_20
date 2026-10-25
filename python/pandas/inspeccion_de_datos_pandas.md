# Inspección de datos — Pandas

Inspecciona un `DataFrame` antes de limpiarlo o analizarlo. Identifica su tamaño, estructura, tipos de datos, valores faltantes y estadísticas básicas.

## Métodos esenciales

```python
import pandas as pd

df = pd.read_csv("datos.csv")

df.head()       # Primeras 5 filas
df.tail()       # Últimas 5 filas
df.info()       # Columnas, tipos y valores no nulos
df.describe()   # Estadísticas descriptivas
df.shape        # Cantidad de filas y columnas
```

## Operaciones frecuentes

```python
df.head(10)             # Primeras 10 filas
df.tail(3)              # Últimas 3 filas
df.columns              # Nombres de las columnas
df.dtypes               # Tipos de datos
df.isna().sum()         # Valores faltantes por columna
df.nunique()            # Valores únicos por columna
df.sample(5)            # 5 filas aleatorias
```

## Interpretación de `shape`

```python
filas, columnas = df.shape

print(f"Filas: {filas}")
print(f"Columnas: {columnas}")
```

`shape` es un atributo, no un método. Accede sin paréntesis.

## Regla 80/20

Ejecuta primero `head()`, `shape`, `info()` e `isna().sum()`. Usa `describe()` para detectar rangos, dispersión y posibles valores atípicos en columnas numéricas.

**Versión de referencia:** Pandas 3.1.0.