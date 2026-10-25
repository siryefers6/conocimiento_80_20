# `strptime()` — Convertir cadenas en fechas y horas

**Python 3.14 · Biblioteca estándar**

Utiliza `datetime.strptime()` para interpretar una cadena según un formato explícito y convertirla en un objeto `datetime`.

## Conversión básica

```python
from datetime import datetime

texto = "24/10/2026 09:30"
fecha = datetime.strptime(texto, "%d/%m/%Y %H:%M")

print(fecha)        # 2026-10-24 09:30:00
print(fecha.year)   # 2026
```

## Directivas de formato frecuentes

| Directiva | Significado           | Ejemplo  |
| --------- | --------------------- | -------- |
| `%d`      | Día (01–31)           | `24`     |
| `%m`      | Mes (01–12)           | `10`     |
| `%Y`      | Año de cuatro dígitos | `2026`   |
| `%y`      | Año de dos dígitos    | `26`     |
| `%H`      | Hora (00–23)          | `09`     |
| `%I`      | Hora (01–12)          | `09`     |
| `%M`      | Minutos               | `30`     |
| `%S`      | Segundos              | `45`     |
| `%p`      | Indicador AM/PM       | `PM`     |
| `%f`      | Microsegundos         | `123456` |
| `%z`      | Desplazamiento UTC    | `-0300`  |

## Convertir solo una fecha o una hora

```python
from datetime import date, datetime, time

fecha = datetime.strptime("24/10/2026", "%d/%m/%Y").date()
hora = datetime.strptime("09:30", "%H:%M").time()
```

`strptime()` devuelve un `datetime`. Extrae `.date()` o `.time()` cuando necesites otro tipo.

## Validar entradas

```python
from datetime import datetime

try:
    fecha = datetime.strptime("31/02/2026", "%d/%m/%Y")
except ValueError:
    print("Fecha inválida")
```

Captura `ValueError` cuando el texto no coincide con el formato o representa una fecha u hora inválida.

## Reglas importantes

- Haz coincidir las directivas con el formato real de entrada.
    
- Usa `%Y` para años de cuatro dígitos.
    
- No confundas `%m` (mes) con `%M` (minutos).
    
- No asumas que `strptime()` valida reglas de negocio, como fechas permitidas o rangos de antigüedad.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html#datetime.datetime.strptime) `[datetime.strptime()](https://docs.python.org/3.14/library/datetime.html#datetime.datetime.strptime)`.