# `resample()` — agregar datos por intervalos temporales

**Pandas 3.1.0**

Usa `resample()` para agrupar datos de series temporales por intervalos regulares, como horas, días, semanas o meses. Requiere una columna temporal convertida a fecha y utilizada como índice, o una columna especificada mediante `on`.

## Agrupar por día

```python
import pandas as pd

df = pd.DataFrame({
    "fecha": pd.to_datetime([
        "2026-10-24 08:00",
        "2026-10-24 12:00",
        "2026-10-25 09:00"
    ]),
    "peso": [100, 150, 200]
})

diario = (
    df.set_index("fecha")
    .resample("D")["peso"]
    .sum()
)

print(diario)
```

Resultado: suma el peso de cada día y genera intervalos diarios, incluidos los días sin registros.

## Frecuencias habituales

| Frecuencia    | Código  |
| ------------- | ------- |
| Minuto        | `"min"` |
| Hora          | `"h"`   |
| Día           | `"D"`   |
| Semana        | `"W"`   |
| Inicio de mes | `"MS"`  |
| Fin de mes    | `"ME"`  |
| Inicio de año | `"YS"`  |

## Patrones esenciales

```python
# Promedio por hora
df.set_index("fecha").resample("h")["peso"].mean()

# Cantidad de registros por día
df.set_index("fecha").resample("D").size()

# Varias métricas por semana
df.set_index("fecha").resample("W").agg(
    peso_total=("peso", "sum"),
    peso_promedio=("peso", "mean"),
    cantidad=("peso", "size")
)

# Alternativa sin establecer el índice
df.resample("D", on="fecha")["peso"].sum()
```

## Conceptos clave

- `resample("D")`: define intervalos diarios.
    
- `.sum()`, `.mean()`, `.count()` y `.size()`: agregan los datos de cada intervalo.
    
- `on="fecha"`: utiliza una columna temporal sin convertirla en índice.
    
- Los intervalos sin datos pueden aparecer con valores ausentes o conteos cero, según la operación aplicada.
    

## Regla práctica

Usa `resample()` para analizar la evolución temporal por períodos regulares. Usa `groupby()` cuando quieras agrupar por categorías; usa `resample()` cuando el criterio principal sea el intervalo de tiempo.