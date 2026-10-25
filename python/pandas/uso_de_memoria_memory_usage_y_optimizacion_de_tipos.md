# Uso de memoria — `memory_usage()` y optimización de tipos

**Pandas 3.1.0**

Inspecciona el consumo de memoria de cada columna y reduce su tamaño seleccionando tipos de datos adecuados.

## 1. Medir el consumo de memoria

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [1001, 1002, 1003],
    "zona": ["Norte", "Sur", "Norte"],
    "peso": [12.5, 18.2, 9.7],
})

print(df.memory_usage(deep=True))
print(df.memory_usage(deep=True).sum())
```

- `deep=True`: incluye una estimación más completa de la memoria usada por objetos, como cadenas.
    
- `sum()`: calcula el consumo total en bytes.
    
- `df.info(memory_usage="deep")`: muestra información del DataFrame y su memoria estimada.
    

## 2. Optimizar tipos numéricos

Usa tipos más pequeños cuando el rango de valores lo permita.

```python
df["pedido"] = df["pedido"].astype("int32")
df["peso"] = df["peso"].astype("float32")
```

Comprueba previamente los valores mínimos y máximos para evitar desbordamientos o pérdida de precisión.

## 3. Optimizar columnas de texto

Si una columna contiene pocas categorías repetidas, conviértela a `category`.

```python
df["zona"] = df["zona"].astype("category")
```

Es especialmente útil cuando existen muchos registros y pocos valores distintos. En columnas con casi todos los valores únicos, puede no ahorrar memoria.

## 4. Comparar antes y después

```python
antes = df.memory_usage(deep=True).sum()

df["zona"] = df["zona"].astype("category")

despues = df.memory_usage(deep=True).sum()

print(f"Antes: {antes:,} bytes")
print(f"Después: {despues:,} bytes")
print(f"Ahorro: {antes - despues:,} bytes")
```

Ejecuta la medición inicial antes de optimizar si necesitas una comparación real.

## Reglas esenciales

- Mide antes de optimizar.
    
- Usa `int32` o `float32` solo si cumplen los requisitos de rango y precisión.
    
- Considera `category` para texto repetido.
    
- No conviertas identificadores a tipos numéricos si deben conservar ceros iniciales.
    
- Verifica que los cambios no alteren los resultados.
    

**Prioriza:** identificar las columnas que más memoria consumen y optimizar primero esas columnas.