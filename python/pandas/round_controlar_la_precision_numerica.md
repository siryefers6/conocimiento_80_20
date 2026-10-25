# `round()` — controlar la precisión numérica

Redondea valores numéricos para presentar resultados con una precisión determinada o preparar reportes.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "peso": [10.567, 20.234, 15.891],
    "volumen": [100.4567, 200.1234, 150.9876],
})

# Redondear todos los valores numéricos a 2 decimales
df_redondeado = df.round(2)

# Redondear una columna específica
df["peso"] = df["peso"].round(1)

# Usar distinta precisión por columna
df = df.round({
    "peso": 1,
    "volumen": 2,
})
```

## Patrones útiles

```python
# Redondear una media
peso_promedio = df["peso"].mean().round(2)

# Redondear a un entero
df["peso_entero"] = df["peso"].round(0)

# Redondear una Serie
df["volumen"] = df["volumen"].round(3)
```

## Reglas 80/20

- `round(decimales)`: define cuántos decimales conservar.
    
- `df.round(2)`: redondea todas las columnas numéricas compatibles.
    
- `df.round({"peso": 1})`: permite establecer la precisión por columna.
    
- El redondeo puede aplicarse a un `DataFrame`, una `Series` o un valor numérico.
    
- `round()` no garantiza una representación decimal exacta para cálculos financieros; utiliza `decimal.Decimal` cuando esa precisión sea necesaria.
    

**Precaución:** redondea preferentemente al presentar resultados, no antes de realizar cálculos que requieran precisión.

**Versión de referencia:** Pandas 3.1.0.