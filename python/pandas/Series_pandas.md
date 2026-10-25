# Series — Pandas

```python
import pandas as pd

serie = pd.Series([10, 20, 30], index=["A", "B", "C"])

print(serie)
print(serie["A"])  # 10
```

- `Series`: estructura unidimensional con etiquetas de índice y valores.
    
- Accede por etiqueta: `serie["A"]`.
    
- Accede por posición: `serie.iloc[0]`.
    
- Consulta el índice: `serie.index`.
    
- Consulta los valores: `serie.to_numpy()`.
    
- Crea desde un diccionario: `pd.Series({"A": 10, "B": 20})`.
    
- Inspecciona los datos: `serie.dtype`, `serie.shape`, `serie.size`.
    

Regla 80/20: domina la creación, el acceso por etiqueta y posición, y la inspección de valores. Una `Series` puede representar una columna de un `DataFrame`.

Versión: Pandas 3.0.x (2026).