# Asignación encadenada — usar `.loc[]`

**Pandas 3.1.0**

Evita modificar un `DataFrame` mediante asignaciones encadenadas. Usa `.loc[filas, columnas]` para indicar explícitamente dónde escribir los valores.

## 1. Evitar asignaciones ambiguas

```python
import pandas as pd

df = pd.DataFrame({
    "peso": [5, 15, 25],
    "estado": ["Pendiente"] * 3,
})

# Evita
df[df["peso"] > 10]["estado"] = "Revisar"

# Correcto
df.loc[df["peso"] > 10, "estado"] = "Revisar"
```

La primera expresión modifica un objeto intermedio y puede generar `SettingWithCopyWarning` en versiones anteriores o `ChainedAssignmentError` en Pandas 3.x.

## 2. Modificar varias columnas

```python
df.loc[df["peso"] > 10, ["estado", "peso"]] = [
    "Revisado", 0
]
```

Usa `.loc[condición, columnas]` para controlar exactamente qué filas y columnas modificar.

## 3. Modificar una copia explícita

Si necesitas trabajar con un subconjunto independiente, crea una copia:

```python
subconjunto = df.loc[df["peso"] > 10].copy()

subconjunto["estado"] = "Revisar"
```

Los cambios en `subconjunto` no modifican `df`.

## Reglas esenciales

- Usa `.loc[filas, columnas]` para asignaciones condicionales.
    
- Usa `.copy()` cuando necesites un `DataFrame` independiente.
    
- No encadenes selección y asignación, por ejemplo: `df[condicion]["columna"] = valor`.
    
- En Pandas 3.x, Copy-on-Write es el comportamiento predeterminado: las modificaciones sobre objetos derivados no actualizan automáticamente el objeto original.
    

**Prioriza:** `.loc[]` para modificar el original y `.copy()` para trabajar independientemente.