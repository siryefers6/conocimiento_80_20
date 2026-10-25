# Conversión de tipos — `astype()` y `to_numeric()`

Convierte columnas al tipo de dato correcto para realizar cálculos, comparar valores y preparar datos para producción.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": ["101", "102", "103"],
    "peso": ["10.5", "20.0", "15.5"],
})

# Convertir una columna a entero
df["pedido"] = df["pedido"].astype("int64")

# Convertir una columna a decimal
df["peso"] = pd.to_numeric(df["peso"])
```

## Manejar valores inválidos

```python
df = pd.DataFrame({
    "peso": ["10.5", "inválido", "20.0"],
})

# Los valores no convertibles se transforman en ausentes (NaN)
df["peso"] = pd.to_numeric(df["peso"], errors="coerce")
```

## Conversiones frecuentes

```python
# Texto
df["zona"] = df["zona"].astype("string")

# Entero que admite valores ausentes
df["pedido"] = df["pedido"].astype("Int64")

# Booleano
df["activo"] = df["activo"].astype("bool")
```

## Reglas 80/20

- `astype(tipo)`: convierte al tipo especificado; falla si los valores no son compatibles.
    
- `pd.to_numeric(columna)`: convierte valores a tipos numéricos.
    
- `errors="coerce"`: convierte valores inválidos en `NaN` para poder identificarlos y tratarlos.
    
- `"Int64"`: tipo entero nullable de Pandas, compatible con valores ausentes.
    
- Verifica los tipos con `df.dtypes`.
    

**Precaución:** `astype("int64")` no admite valores ausentes. Limpia los datos o utiliza `"Int64"` cuando necesites conservarlos.

**Versión de referencia:** Pandas 3.1.0.