# Excel — `read_excel()` y `to_excel()` en Pandas

Importa informes de Excel con `pd.read_excel()` y exporta resultados con `DataFrame.to_excel()`. Utiliza `openpyxl` para trabajar con archivos `.xlsx`.

## 1. Leer un archivo Excel

```python
import pandas as pd

df = pd.read_excel("informe.xlsx")
```

Instala el motor de lectura si es necesario:

```
uv add openpyxl
```

## 2. Seleccionar hojas y columnas

```python
df = pd.read_excel(
    "informe.xlsx",
    sheet_name="Datos",
    usecols=["Pedido", "Supervisor", "Ruta"],
)
```

- `sheet_name=0`: lee la primera hoja.
    
- `sheet_name="Datos"`: selecciona una hoja por nombre.
    
- `sheet_name=None`: carga todas las hojas en un diccionario de `DataFrame`.
    
- `skiprows=2`: omite las dos primeras filas.
    
- `nrows=100`: lee como máximo 100 filas.
    

## 3. Exportar un archivo Excel

```python
df.to_excel(
    "resultado.xlsx",
    sheet_name="Resultados",
    index=False,
)
```

- `index=False`: evita exportar el índice como columna.
    
- `columns=["Pedido", "Ruta"]`: exporta solo las columnas indicadas.
    
- `na_rep=""`: representa los valores ausentes con celdas vacías.
    

## 4. Exportar varias hojas

```python
with pd.ExcelWriter("informe.xlsx", engine="openpyxl") as writer:
    df.to_excel(writer, sheet_name="Datos", index=False)
    df.describe().to_excel(writer, sheet_name="Resumen")
```

Usa `ExcelWriter` para generar varias hojas en un mismo archivo.

## Regla 80/20

Domina `read_excel()`, `to_excel()`, `sheet_name`, `usecols`, `index=False` y `ExcelWriter`. Inspecciona los datos después de importarlos para detectar columnas incorrectas y tipos inesperados.

**Versión de referencia:** Pandas 3.1.0.