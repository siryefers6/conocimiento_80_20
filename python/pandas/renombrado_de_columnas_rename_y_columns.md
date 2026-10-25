# Renombrado de columnas — `rename()` y `columns`

Cambia los nombres de las columnas para mejorar la legibilidad, normalizar datos importados y mantener una estructura consistente.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "N° Pedido": [101, 102],
    "Peso (kg)": [15.5, 20.0],
})

# Renombrar columnas específicas
df = df.rename(columns={
    "N° Pedido": "numero_pedido",
    "Peso (kg)": "peso_kg",
})

# Consultar los nombres actuales
print(df.columns)
```

## Operaciones esenciales

```python
# Convertir todos los nombres a minúsculas
df.columns = df.columns.str.lower()

# Reemplazar espacios por guiones bajos
df.columns = df.columns.str.replace(" ", "_", regex=False)

# Asignar todos los nombres en el orden actual
df.columns = ["pedido", "peso"]
```

## Reglas 80/20

- `rename(columns={...})`: renombra columnas específicas sin modificar las demás.
    
- `df.columns`: consulta los nombres actuales.
    
- `df.columns = [...]`: reemplaza todos los nombres; la lista debe tener la misma longitud que el número de columnas.
    
- Normaliza los nombres al importar datos para facilitar su uso en el resto del proyecto.
    

**Prioriza:** `rename()` para cambios puntuales y `columns.str` para normalizaciones sistemáticas.

**Versión de referencia:** Pandas 3.1.0.