# Filtrado booleano — Pandas

Filtra filas mediante condiciones que producen valores booleanos (`True` o `False`). Combina condiciones con `&` (AND), `|` (OR) y `~` (NOT).

## 1. Condición simple

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [101, 102, 103, 104],
    "cantidad": [5, 20, 15, 8],
    "zona": ["Norte", "Sur", "Norte", "Sur"],
})

df[df["cantidad"] > 10]
```

## 2. Combinar condiciones

```python
# AND: ambas condiciones deben cumplirse
df[(df["cantidad"] > 10) & (df["zona"] == "Norte")]

# OR: al menos una condición debe cumplirse
df[(df["cantidad"] > 15) | (df["zona"] == "Sur")]

# NOT: invierte la condición
df[~(df["zona"] == "Norte")]
```

## 3. Operadores esenciales

| Operador | Función | Ejemplo                                            |
| -------- | ------- | -------------------------------------------------- |
| `&`      | AND     | `(df["cantidad"] > 5) & (df["cantidad"] < 20)`     |
| `\|`     | OR      | `(df["zona"] == "Norte") \| (df["zona"] == "Sur")` |
| `~`      | NOT     | `~(df["cantidad"] > 10)`                           |

**Importante:** utiliza `&`, `|` y `~` en lugar de `and`, `or` y `not`. Encierra cada condición entre paréntesis.

## 4. Filtrar valores habituales

```python
df[df["zona"].isin(["Norte", "Sur"])]
df[df["cantidad"].between(5, 15)]
df[df["zona"].isna()]
df[df["zona"].notna()]
```

- `isin()`: comprueba pertenencia a una lista.
    
- `between()`: comprueba si un valor está dentro de un intervalo inclusivo.
    
- `isna()` y `notna()`: identifican valores ausentes y presentes.
    

## Regla 80/20

Domina las comparaciones (`==`, `!=`, `>`, `<`), los operadores `&`, `|`, `~` y los métodos `isin()` e `isna()`. Aplica estos patrones para filtrar informes y conjuntos de datos reales.

**Versión de referencia:** Pandas 3.1.0.