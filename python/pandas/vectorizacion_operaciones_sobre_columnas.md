# Vectorización — operaciones sobre columnas

**Pandas 3.1.0**

Usa operaciones vectorizadas para procesar columnas completas sin recorrer filas mediante bucles Python. Generalmente, son más rápidas y legibles.

## 1. Evitar bucles

```python
import pandas as pd

df = pd.DataFrame({"precio": [100, 200, 300]})

# Evita
df["con_iva"] = [
    precio * 1.19 for precio in df["precio"]
]

# Preferible
df["con_iva"] = df["precio"] * 1.19
```

## 2. Aplicar condiciones

```python
df["categoria"] = "Normal"

df.loc[df["precio"] >= 200, "categoria"] = "Alto"
```

Para condiciones simples, también puedes usar `where()` o `numpy.select()`.

## 3. Operar entre columnas

```python
df["total"] = df["precio"] * df["cantidad"]
df["descuento"] = df["total"] * 0.10
```

## 4. Evitar `apply()` cuando exista una alternativa vectorizada

```python
# Menos eficiente
df["doble"] = df["precio"].apply(lambda x: x * 2)

# Preferible
df["doble"] = df["precio"] * 2
```

`apply()` sigue siendo útil para lógica compleja sin una alternativa vectorizada directa.

## Reglas esenciales

- Opera directamente sobre `Series` y `DataFrame`.
    
- Evita bucles con `iterrows()` y `itertuples()` cuando puedas expresar la operación por columnas.
    
- Usa `loc[]` para actualizar filas según condiciones.
    
- Mide el rendimiento con datos representativos antes de optimizar.
    

**Prioriza:** operaciones aritméticas, comparaciones, máscaras booleanas y funciones integradas de Pandas.