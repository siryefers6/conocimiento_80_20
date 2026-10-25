# Tipos de datos (`dtypes`) — Pandas

Identifica y controla los tipos de datos de cada columna. Usa tipos adecuados para calcular, filtrar, comparar fechas y gestionar valores ausentes.

## 1. Identificar tipos

```python
df.dtypes             # Tipo de cada columna
df["precio"].dtype    # Tipo de una columna
df.info()             # Tipos y valores no nulos
```

## 2. Convertir tipos

```python
df["precio"] = df["precio"].astype("float64")
df["cantidad"] = df["cantidad"].astype("int64")
df["nombre"] = df["nombre"].astype("string")
df["fecha"] = pd.to_datetime(df["fecha"], errors="coerce")
```

Usa `errors="coerce"` para convertir valores inválidos en ausentes (`NaT` en fechas).

## 3. Tipos habituales

| Tipo             | Uso                                                |
| ---------------- | -------------------------------------------------- |
| `int64`          | Enteros                                            |
| `float64`        | Números decimales                                  |
| `bool`           | Valores booleanos                                  |
| `string`         | Texto                                              |
| `datetime64[ns]` | Fechas y horas                                     |
| `category`       | Valores categóricos repetidos                      |
| `object`         | Datos heterogéneos o tipos tradicionales de Python |
| `Int64`          | Enteros que admiten `pd.NA`                        |
| `boolean`        | Booleanos que admiten `pd.NA`                      |

## 4. Gestionar valores ausentes

```python
df.isna().sum()                       # Contar ausentes
df["cantidad"] = df["cantidad"].astype("Int64")
df["precio"] = pd.to_numeric(df["precio"], errors="coerce")
df["fecha"] = pd.to_datetime(df["fecha"], errors="coerce")
```

Distingue `NaN`, `NaT` y `pd.NA`: representan ausencia de datos en contextos numéricos, temporales y tipos anulables, respectivamente.

## Regla 80/20

Domina `dtypes`, `astype()`, `pd.to_numeric()`, `pd.to_datetime()` e `isna()`. Comprueba los tipos después de importar archivos y antes de realizar cálculos.

**Versión de referencia:** Pandas 3.1.0.