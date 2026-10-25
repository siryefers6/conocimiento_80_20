# Limpieza de texto — `.str.strip()`, `.str.lower()`, `.str.replace()` y `.str.contains()`

Usa el accesor `.str` para limpiar, normalizar y buscar texto en columnas de un `DataFrame`.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "zona": [" Norte ", "SUR", "Centro  ", None],
})

# Eliminar espacios al inicio y al final
df["zona"] = df["zona"].str.strip()

# Convertir a minúsculas
df["zona"] = df["zona"].str.lower()

# Reemplazar texto
df["zona"] = df["zona"].str.replace("sur", "sur-oriente", regex=False)

# Filtrar filas que contienen un texto
resultado = df[df["zona"].str.contains("norte", na=False)]
```

## Operaciones esenciales

- `.str.strip()`: elimina espacios al inicio y al final.
    
- `.str.lower()`: convierte el texto a minúsculas para normalizar comparaciones.
    
- `.str.replace(antiguo, nuevo, regex=False)`: reemplaza texto literal.
    
- `.str.contains(texto, na=False)`: identifica filas cuyo texto contiene una coincidencia; `na=False` trata los valores ausentes como `False`.
    

## Patrones útiles

```python
# Eliminar espacios repetidos entre palabras
df["zona"] = df["zona"].str.replace(r"\s+", " ", regex=True)

# Buscar sin distinguir mayúsculas y minúsculas
resultado = df[df["zona"].str.contains("norte", case=False, na=False)]

# Convertir a mayúsculas
df["zona"] = df["zona"].str.upper()
```

## Reglas 80/20

- Normaliza antes de comparar: `.str.strip().str.lower()`.
    
- Usa `regex=False` cuando busques o reemplaces texto literal.
    
- Usa `regex=True` cuando necesites patrones, como `r"\s+"`.
    
- Usa `na=False` al filtrar columnas que pueden contener valores ausentes.
    

**Precaución:** estos métodos devuelven valores nuevos; asigna el resultado a la columna para conservar los cambios.

**Versión de referencia:** Pandas 3.1.0.