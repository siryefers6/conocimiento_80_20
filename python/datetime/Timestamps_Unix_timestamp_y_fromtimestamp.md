# Timestamps Unix — `timestamp()` y `fromtimestamp()`

**Python 3.14 · Biblioteca estándar**

Convierte fechas y horas a timestamps Unix y viceversa. Un timestamp representa los segundos transcurridos desde la época Unix: `1970-01-01 00:00:00 UTC`.

## Convertir una fecha a timestamp

```python
from datetime import UTC, datetime

fecha = datetime(2026, 10, 24, 12, 0, tzinfo=UTC)

timestamp = fecha.timestamp()

print(timestamp)  # Segundos desde la época Unix
```

Usa objetos `datetime` _aware_ para evitar ambigüedades al convertir instantes.

## Convertir un timestamp a fecha

```python
from datetime import UTC, datetime

timestamp = 1792843200

fecha_utc = datetime.fromtimestamp(timestamp, UTC)

print(fecha_utc)
```

Pasa `UTC` explícitamente para obtener un objeto _aware_ en UTC, independientemente de la zona horaria local del sistema.

## Obtener la fecha local

```python
from datetime import datetime

timestamp = datetime.now().timestamp()
fecha_local = datetime.fromtimestamp(timestamp)

print(fecha_local)
```

Sin el argumento de zona horaria, `fromtimestamp()` devuelve la fecha y hora local como objeto _naive_.

## Reglas importantes

- Usa `timestamp()` para obtener segundos Unix, incluidos los posibles decimales.
    
- Usa `datetime.fromtimestamp(valor, UTC)` para reconstruir un instante en UTC.
    
- No uses `datetime.timestamp()` sobre objetos _naive_ si desconoces qué zona horaria representan: Python los interpreta como hora local.
    
- No confundas segundos Unix con milisegundos Unix; divide los milisegundos entre `1000` antes de convertirlos.
    
- Considera que el rango admitido depende de la plataforma y puede producir `OverflowError` u `OSError`.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html#datetime.datetime.timestamp) `[timestamp()](https://docs.python.org/3.14/library/datetime.html#datetime.datetime.timestamp)` [y](https://docs.python.org/3.14/library/datetime.html#datetime.datetime.timestamp) `[fromtimestamp()](https://docs.python.org/3.14/library/datetime.html#datetime.datetime.timestamp)`.