# Copy-on-Write — copias y modificaciones en Pandas 3.x

**Pandas 3.1.0**

Copy-on-Write (CoW) garantiza que modificar un `DataFrame` derivado no altere accidentalmente el objeto original. Pandas comparte datos internamente cuando es posible y crea copias cuando una modificación lo requiere.

## 1. Modificar un subconjunto

```python
import pandas as pd

df = pd.DataFrame({"peso": [10, 20, 30]})

subconjunto = df["peso"]

subconjunto.iloc[0] = 99

print(df["peso"].tolist())  # [10, 20, 30]
```

La modificación afecta a `subconjunto`, no a `df`.

## 2. Modificar el original con `.loc[]`

```python
df.loc[df["peso"] > 15, "peso"] = 0

print(df["peso"].tolist())  # [10, 0, 0]
```

Usa `.loc[]` cuando quieras modificar directamente el `DataFrame` original.

## 3. Crear una copia independiente

```python
copia = df.copy()

copia["peso"] = copia["peso"] * 2
```

Los cambios en `copia` no afectan a `df`. Usa `.copy()` para expresar explícitamente que necesitas un objeto independiente.

## 4. Evitar asignaciones encadenadas

```python
# Incorrecto: no modifica df
df[df["peso"] > 10]["peso"] = 5

# Correcto: modifica df
df.loc[df["peso"] > 10, "peso"] = 5
```

La asignación encadenada no actualiza el objeto original bajo Copy-on-Write.

## Reglas esenciales

- Pandas 3.x utiliza Copy-on-Write como comportamiento predeterminado.
    
- Modificar un objeto derivado no modifica automáticamente su origen.
    
- Usa `.loc[]` para modificar el objeto original.
    
- Usa `.copy()` cuando necesites una copia independiente explícita.
    
- No dependas de que una selección comparta memoria con el objeto original.
    

**Prioriza:** identifica qué objeto necesitas modificar antes de realizar una asignación.