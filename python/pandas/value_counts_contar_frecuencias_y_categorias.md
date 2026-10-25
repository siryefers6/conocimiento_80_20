# `value_counts()` — contar frecuencias y categorías

Cuenta cuántas veces aparece cada valor único en una `Series`. Úsalo para analizar categorías, detectar valores inesperados y resumir datos operacionales.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "zona": ["Norte", "Sur", "Norte", "Centro", "Sur", "Norte"],
})

# Contar apariciones por categoría
frecuencias = df["zona"].value_counts()

# Contar incluyendo valores ausentes
frecuencias = df["zona"].value_counts(dropna=False)

# Obtener proporciones
proporciones = df["zona"].value_counts(normalize=True)

# Obtener los 2 valores más frecuentes
top_zonas = df["zona"].value_counts().head(2)
```

## Parámetros esenciales

- `normalize=True`: devuelve proporciones en lugar de cantidades.
    
- `dropna=False`: incluye los valores ausentes en el conteo.
    
- `ascending=True`: ordena de menor a mayor frecuencia; por defecto, ordena de mayor a menor.
    
- `sort=False`: conserva el orden de aparición de los valores únicos, en lugar de ordenar por frecuencia.
    

## Patrones útiles

```python
# Frecuencias porcentuales
porcentajes = df["zona"].value_counts(normalize=True) * 100

# Contar combinaciones de categorías
combinaciones = df.value_counts(["zona"])

# Contar categorías de una columna
cantidad_categorias = df["zona"].nunique()
```

## Reglas 80/20

- Usa `value_counts()` para identificar categorías frecuentes.
    
- Usa `normalize=True` para calcular proporciones.
    
- Usa `dropna=False` para detectar valores ausentes dentro de la distribución.
    
- Usa `nunique()` para contar cuántos valores únicos existen, sin contar ausentes por defecto.
    
- Recuerda que `value_counts()` devuelve una `Series` ordenada por frecuencia descendente de forma predeterminada.
    

**Versión de referencia:** Pandas 3.1.0.