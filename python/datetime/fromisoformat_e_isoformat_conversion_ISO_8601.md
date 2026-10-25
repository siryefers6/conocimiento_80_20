# `fromisoformat()` e `isoformat()` — Conversión ISO 8601

**Python 3.14 · Biblioteca estándar**

Utiliza `fromisoformat()` para convertir cadenas ISO 8601 en objetos de fecha y hora, e `isoformat()` para generar cadenas a partir de esos objetos.

## Convertir cadenas a objetos

```python
from datetime import date, datetime, time

fecha = date.fromisoformat("2026-10-24")
hora = time.fromisoformat("09:30:00")
fecha_hora = datetime.fromisoformat("2026-10-24T09:30:00")
```

## Convertir objetos a cadenas

```python
from datetime import UTC, datetime

ahora = datetime.now(UTC)

texto = ahora.isoformat()
print(texto)  # Ejemplo: 2026-10-24T12:30:00+00:00
```

## Formatos habituales

| Formato          | Ejemplo                     |
| ---------------- | --------------------------- |
| Fecha            | `2026-10-24`                |
| Hora             | `09:30:00`                  |
| Fecha y hora     | `2026-10-24T09:30:00`       |
| Fecha y hora UTC | `2026-10-24T12:30:00+00:00` |

## Validar entradas

```python
from datetime import datetime

try:
    fecha = datetime.fromisoformat("2026-10-24T09:30:00")
except ValueError:
    print("Fecha u hora inválida")
```

## Reglas importantes

- Usa `fromisoformat()` para interpretar formatos ISO 8601 admitidos por Python.
    
- Usa `isoformat()` para serializar fechas y horas en cadenas.
    
- Conserva el desplazamiento UTC cuando necesites representar una zona horaria.
    
- No confundas una fecha y hora sin zona horaria con una que incluye desplazamiento.
    
- No asumas que `fromisoformat()` acepta cualquier formato de texto; utiliza `strptime()` para formatos personalizados.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html) `[datetime](https://docs.python.org/3.14/library/datetime.html)`.