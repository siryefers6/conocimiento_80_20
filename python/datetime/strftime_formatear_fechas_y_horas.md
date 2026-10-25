# `strftime()` — Formatear fechas y horas

**Python 3.14 · Biblioteca estándar**

Utiliza `strftime()` para convertir objetos `date`, `time` o `datetime` en cadenas con un formato específico.

## Uso básico

```python
from datetime import datetime

ahora = datetime(2026, 10, 24, 9, 30, 45)

print(ahora.strftime("%d/%m/%Y"))       # 24/10/2026
print(ahora.strftime("%H:%M:%S"))       # 09:30:45
print(ahora.strftime("%d/%m/%Y %H:%M")) # 24/10/2026 09:30
```

## Directivas frecuentes

| Directiva | Significado              | Ejemplo    |
| --------- | ------------------------ | ---------- |
| `%d`      | Día del mes              | `24`       |
| `%m`      | Mes numérico             | `10`       |
| `%Y`      | Año de cuatro dígitos    | `2026`     |
| `%y`      | Año de dos dígitos       | `26`       |
| `%H`      | Hora (00–23)             | `09`       |
| `%I`      | Hora (01–12)             | `09`       |
| `%M`      | Minutos                  | `30`       |
| `%S`      | Segundos                 | `45`       |
| `%A`      | Nombre completo del día  | `Saturday` |
| `%B`      | Nombre completo del mes  | `October`  |
| `%a`      | Nombre abreviado del día | `Sat`      |
| `%b`      | Nombre abreviado del mes | `Oct`      |
| `%p`      | Indicador AM/PM          | `AM`       |

## Formatear la fecha actual

```python
from datetime import datetime

ahora = datetime.now()

fecha_archivo = ahora.strftime("%Y-%m-%d_%H-%M-%S")
print(fecha_archivo)  # Ejemplo: 2026-10-24_09-30-45
```

Usa este formato para generar nombres de archivos ordenables cronológicamente.

## Reglas importantes

- Usa `%Y-%m-%d` para representar fechas de forma ordenable: `2026-10-24`.
    
- Distingue `%m` (mes) de `%M` (minutos).
    
- Ten en cuenta que los nombres de días y meses dependen de la configuración regional.
    
- No uses `strftime()` para validar entradas; utiliza `strptime()`.
    
- No asumas que todos los códigos de formato se comportan igual en todos los sistemas operativos.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html#strftime-and-strptime-format-codes) `[strftime()](https://docs.python.org/3.14/library/datetime.html#strftime-and-strptime-format-codes)` [y](https://docs.python.org/3.14/library/datetime.html#strftime-and-strptime-format-codes) `[strptime()](https://docs.python.org/3.14/library/datetime.html#strftime-and-strptime-format-codes)`.