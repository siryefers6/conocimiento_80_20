# `apply()` — aplicar funciones personalizadas

**Pandas 3.1.0**

Usa `apply()` para aplicar una función a cada elemento o fila cuando no exista una alternativa vectorizada sencilla. Prefiere operaciones vectorizadas cuando sea posible.

## 1. Aplicar una función a una columna

```python
import pandas as pd

df = pd.DataFrame({"precio": [100, 250, 500]})

def clasificar(precio):
    return "Alto" if precio >= 250 else "Normal"

df["categoria"] = df["precio"].apply(clasificar)
```

Para funciones simples, también puedes usar `lambda`:

```python
df["doble"] = df["precio"].apply(lambda x: x * 2)
```

Si la operación es aritmética, prefiere `df["precio"] * 2`.

## 2. Aplicar una función por fila

Usa `axis=1` para recibir cada fila como una `Series`.

```python
df = pd.DataFrame({
    "precio": [100, 200],
    "cantidad": [3, 5],
})

df["total"] = df.apply(
    lambda fila: fila["precio"] * fila["cantidad"],
    axis=1,
)
```

En este caso, es preferible la operación vectorizada:

```python
df["total"] = df["precio"] * df["cantidad"]
```

## 3. Aplicar una función a cada columna

Con `axis=0`, la función recibe cada columna como una `Series`.

```python
df.apply(lambda columna: columna.max() - columna.min())
```

## Reglas esenciales

- `Series.apply(func)` aplica una función a los valores de una columna.
    
- `DataFrame.apply(func, axis=1)` procesa filas.
    
- `DataFrame.apply(func, axis=0)` procesa columnas.
    
- Evita `axis=1` en archivos grandes si puedes expresar el cálculo con operaciones vectorizadas.
    
- No confundas `apply()` con `map()`: `Series.map()` es útil para transformar valores mediante diccionarios o funciones.
    

**Prioriza:** vectorización primero; `apply()` cuando la lógica personalizada lo justifique.