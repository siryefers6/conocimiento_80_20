# Obtener la fecha y hora actuales en Python

**Python 3.14 · Biblioteca estándar**

Utiliza `datetime.now()`, `date.today()` y `datetime.now(UTC)` para obtener la fecha y hora local o UTC.

## Obtener valores actuales

```python
from datetime import UTC, date, datetime

ahora = datetime.now()       # Fecha y hora local, sin zona explícita
hoy = date.today()           # Fecha local
ahora_utc = datetime.now(UTC)  # Fecha y hora con zona UTC

print(ahora)
print(hoy)
print(ahora_utc)
```

## Diferencias importantes

- `datetime.now()`: devuelve la fecha y hora local sin información explícita de zona horaria (_naive_).
    
- `date.today()`: devuelve la fecha local sin hora.
    
- `datetime.now(UTC)`: devuelve un objeto con zona horaria UTC (_aware_).
    

## Reglas de uso

- Usa `date.today()` para fechas locales de registro o vencimiento.
    
- Usa `datetime.now()` cuando necesites la hora local y no requieras una zona horaria explícita.
    
- Usa `datetime.now(UTC)` para registros temporales consistentes entre sistemas.
    
- Evita mezclar objetos _naive_ y _aware_ en comparaciones o restas: Python puede generar `TypeError`.
    
- Prefiere `UTC` frente a `timezone.utc` en código Python 3.14.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html) `[datetime](https://docs.python.org/3.14/library/datetime.html)`.