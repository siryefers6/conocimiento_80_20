# Operaciones vectorizadas — calcular directamente sobre columnas

Aplica operaciones a columnas completas sin recorrer filas con bucles. Pandas delega los cálculos en operaciones optimizadas de NumPy cuando corresponde.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [101, 102, 103],
    "peso": [10, 20, 30],
    "cantidad": [2, 3, 4],
})

# Operaciones aritméticas
df["peso_doble"] = df["peso"] * 2
df["peso_total"] = df["peso"] * df["cantidad"]

# Condiciones booleanas
df["es_pesado"] = df["peso"] >= 20

# Operaciones entre columnas
df["peso_promedio"] = df["peso_total"] / df["cantidad"]
```

## Patrones útiles

```python
# Filtrar sin recorrer filas
pesados = df[df["peso"] >= 20]

# Aplicar una transformación condicional
df["categoria"] = "Normal"
df.loc[df["peso"] >= 20, "categoria"] = "Pesado"

# Calcular estadísticas de una columna
peso_total = df["peso"].sum()
peso_promedio = df["peso"].mean()
peso_maximo = df["peso"].max()
```

## Reglas 80/20

- Usa operadores aritméticos (`+`, `-`, `*`, `/`) directamente sobre columnas.
    
- Combina condiciones con `&`, `|` y `~`; utiliza paréntesis alrededor de cada condición.
    
- Usa `loc[]` para asignaciones condicionales.
    
- Evita `iterrows()` y los bucles cuando una operación vectorizada resuelva el problema.
    
- Recuerda que la alineación entre `Series` se realiza por etiquetas del índice, no necesariamente por posición.
    

**Prioriza:** operaciones vectorizadas para cálculos; `apply()` solo cuando no exista una alternativa vectorizada clara.

**Versión de referencia:** Pandas 3.1.0.