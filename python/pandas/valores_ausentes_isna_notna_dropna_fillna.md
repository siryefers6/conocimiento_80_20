# Valores ausentes — `isna()`, `notna()`, `dropna()` y `fillna()`

Detecta, elimina o reemplaza valores ausentes (`NaN`, `None`, `NaT`) antes de analizar los datos.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [101, 102, 103],
    "peso": [10.5, None, 20.0],
    "zona": ["Norte", None, "Sur"],
})

# Detectar valores ausentes
df.isna()

# Detectar valores presentes
df.notna()

# Contar ausentes por columna
df.isna().sum()

# Eliminar filas con algún valor ausente
df_limpio = df.dropna()

# Reemplazar ausentes por un valor fijo
df_relleno = df.fillna({"peso": 0, "zona": "Sin zona"})
```

## Operaciones esenciales

- `isna()`: devuelve `True` donde falta un valor.
    
- `notna()`: devuelve `True` donde existe un valor.
    
- `dropna()`: elimina filas o columnas con valores ausentes; por defecto elimina filas que contienen al menos uno.
    
- `fillna(valor)`: reemplaza valores ausentes con un valor definido.
    

## Patrones útiles

```python
# Eliminar filas donde falte el peso
df = df.dropna(subset=["peso"])

# Eliminar solo filas completamente vacías
df = df.dropna(how="all")

# Rellenar con la mediana de la columna
df["peso"] = df["peso"].fillna(df["peso"].median())
```

**Reglas 80/20**

- Cuenta los ausentes con `df.isna().sum()`.
    
- Usa `dropna(subset=[...])` cuando una columna sea obligatoria.
    
- Usa `fillna()` cuando puedas justificar el valor de reemplazo.
    
- No reemplaces automáticamente todos los ausentes por `0`: puede alterar los resultados.
    

**Versión de referencia:** Pandas 3.1.0.