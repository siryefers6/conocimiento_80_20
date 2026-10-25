# Creación de estructuras — Pandas

Construye objetos `Series` y `DataFrame` desde listas, diccionarios y otras fuentes de datos.

## 1. Crear un DataFrame desde un diccionario

Usa las claves como columnas y las listas como valores.

```
import pandas as pd

df = pd.DataFrame({
    "producto": ["Teclado", "Ratón"],
    "precio": [25000, 12000],
})

print(df)
```

## 2. Crear desde una lista de diccionarios

Usa cada diccionario como una fila.

```
datos = [
    {"producto": "Teclado", "precio": 25000},
    {"producto": "Ratón", "precio": 12000},
]

df = pd.DataFrame(datos)
```

## 3. Crear desde una lista de listas

Define los nombres de las columnas explícitamente.

```python
datos = [
    ["Teclado", 25000],
    ["Ratón", 12000],
]

df = pd.DataFrame(
    datos,
    columns=["producto", "precio"],
)
```

## 4. Crear una Series

Usa `pd.Series()` para representar una secuencia unidimensional.

```python
precios = pd.Series(
    [25000, 12000],
    index=["Teclado", "Ratón"],
)
```

## 5. Leer archivos

Carga datos desde fuentes externas.

```python
df = pd.read_csv("datos.csv")
df = pd.read_excel("datos.xlsx")
df = pd.read_json("datos.json")
```

## Regla 80/20

Prioriza estas funciones:

- `pd.DataFrame()`: construir tablas desde estructuras de Python.
    
- `pd.Series()`: construir secuencias etiquetadas.
    
- `pd.read_csv()`: importar archivos CSV.
    
- `pd.read_excel()`: importar archivos Excel.
    
- `pd.read_json()`: importar datos JSON.
    

**Versión de referencia:** Pandas 3.1.0.