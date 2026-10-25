# Parámetros de importación — Pandas

Configura la lectura de archivos para interpretar correctamente separadores, codificación, encabezados, columnas y tipos de datos. Aplica estos parámetros principalmente con `pd.read_csv()`.

## 1. Separadores y codificación

```python
df = pd.read_csv(
    "datos.csv",
    sep=";",
    encoding="utf-8",
    decimal=",",
    thousands=".",
)
```

- `sep`: define el delimitador de columnas.
    
- `encoding`: especifica la codificación del archivo.
    
- `decimal`: define el separador decimal.
    
- `thousands`: identifica el separador de miles.
    

## 2. Encabezados y filas

```python
df = pd.read_csv(
    "datos.csv",
    header=0,
    skiprows=2,
    nrows=1000,
)
```

- `header=0`: utiliza la primera fila leída como encabezado.
    
- `header=None`: indica que el archivo no contiene encabezados.
    
- `names=["pedido", "cantidad"]`: asigna nombres a las columnas.
    
- `skiprows=2`: omite las dos primeras filas.
    
- `nrows=1000`: limita la lectura a 1.000 filas.
    

## 3. Seleccionar columnas y controlar tipos

```python
df = pd.read_csv(
    "datos.csv",
    usecols=["pedido", "cantidad", "fecha"],
    dtype={"pedido": "string", "cantidad": "Int64"},
    parse_dates=["fecha"],
)
```

- `usecols`: importa únicamente las columnas necesarias.
    
- `dtype`: establece tipos de datos explícitos.
    
- `parse_dates`: intenta interpretar las columnas como fechas.
    

Para conversiones más controladas, utiliza `pd.to_datetime()` después de importar.

## 4. Gestionar valores ausentes

```python
df = pd.read_csv(
    "datos.csv",
    na_values=["N/A", "SIN DATO", "-"],
    keep_default_na=True,
)
```

- `na_values`: define valores adicionales que deben interpretarse como ausentes.
    
- `keep_default_na`: conserva el reconocimiento predeterminado de valores ausentes.
    

## 5. Inspeccionar el resultado

```python
print(df.head())
print(df.shape)
print(df.dtypes)
print(df.isna().sum())
```

Verifica que las columnas, los tipos y los valores ausentes coincidan con lo esperado.

## Regla 80/20

Prioriza `sep`, `encoding`, `header`, `usecols`, `dtype`, `parse_dates` y `na_values`. Configura la importación correctamente para reducir las correcciones posteriores.

**Versión de referencia:** Pandas 3.1.0.