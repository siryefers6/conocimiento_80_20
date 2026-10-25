# `shift()` y `diff()` — comparar registros consecutivos

**Pandas 3.1.0**

Usa `shift()` para acceder al valor de una fila anterior o siguiente y `diff()` para calcular la diferencia entre valores consecutivos. Son útiles para analizar cambios, variaciones y tiempos entre eventos.

## `shift()` — desplazar valores

```python
import pandas as pd

df = pd.DataFrame({
    "fecha": pd.to_datetime([
        "2026-10-24 08:00",
        "2026-10-24 09:00",
        "2026-10-24 11:00"
    ]),
    "peso": [100, 130, 120]
})

df["peso_anterior"] = df["peso"].shift(1)
df["peso_siguiente"] = df["peso"].shift(-1)
```

- `shift(1)`: obtiene el valor de la fila anterior.
    
- `shift(-1)`: obtiene el valor de la fila siguiente.
    
- El valor desplazado que queda sin correspondencia será `NaN`.
    

## `diff()` — calcular diferencias

```python
df["variacion"] = df["peso"].diff()
df["tiempo_transcurrido"] = df["fecha"].diff()
```

Resultado conceptual:

| Peso | Peso anterior | Variación |
| ---- | ------------- | --------- |
| 100  | `NaN`         | `NaN`     |
| 130  | 100           | 30        |
| 120  | 130           | -10       |

`diff()` calcula el valor actual menos el valor anterior. Con fechas, devuelve una duración (`Timedelta`).

## Patrones prácticos

```python
# Variación porcentual respecto al registro anterior
df["variacion_pct"] = df["peso"].pct_change() * 100

# Tiempo transcurrido en minutos
df["minutos"] = df["fecha"].diff().dt.total_seconds() / 60

# Comparar si el valor aumentó
df["aumento"] = df["peso"] > df["peso"].shift(1)
```

## Reglas prácticas

- Ordena primero por fecha si el análisis es temporal: `df.sort_values("fecha")`.
    
- Usa `shift()` para comparar valores o recuperar datos de filas cercanas.
    
- Usa `diff()` para calcular diferencias absolutas.
    
- Usa `pct_change()` para calcular variaciones relativas.
    
- Si existen varios pedidos o grupos independientes, calcula las diferencias dentro de cada grupo con `df.groupby("pedido_id")["peso"].diff()`.
    

**Prioriza:** `shift(1)`, `diff()` y `pct_change()` para detectar cambios entre registros consecutivos.