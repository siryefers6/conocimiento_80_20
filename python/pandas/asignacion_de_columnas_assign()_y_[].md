# Asignación de columnas — `assign()` y `[]`

Crea o modifica columnas calculadas para transformar datos sin recorrer filas manualmente.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [101, 102, 103],
    "peso": [10, 20, 30],
})

# Crear una columna mediante asignación directa
df["peso_kg"] = df["peso"] / 1000

# Crear varias columnas con assign()
resultado = df.assign(
    peso_doble=df["peso"] * 2,
    peso_valido=df["peso"] > 15,
)
```

## Diferencias esenciales

- `df["columna"] = valor`: crea una columna o reemplaza sus valores.
    
- `df.assign(columna=valor)`: devuelve un nuevo `DataFrame` con la columna añadida o modificada; no altera el original por defecto.
    
- Ambas operaciones permiten cálculos vectorizados, sin usar bucles.
    

## Patrones útiles

```python
# Crear una columna a partir de otras
df["peso_doble"] = df["peso"] * 2

# Crear columnas encadenando transformaciones
resultado = (
    df.assign(peso_doble=lambda x: x["peso"] * 2)
      .assign(es_pesado=lambda x: x["peso_doble"] >= 40)
)

# Asignar un valor constante
df["estado"] = "pendiente"
```

## Reglas 80/20

- Prioriza `[]` para asignaciones simples y directas.
    
- Usa `assign()` para encadenar transformaciones sin modificar el `DataFrame` original.
    
- Aprovecha operaciones vectorizadas como `*`, `/`, `+` y comparaciones.
    
- Con `assign()`, usa `lambda x: ...` cuando una columna nueva dependa de otra creada en la misma cadena.
    

**Versión de referencia:** Pandas 3.1.0.