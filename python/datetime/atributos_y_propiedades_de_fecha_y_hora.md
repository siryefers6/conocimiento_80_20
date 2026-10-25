# Atributos y propiedades de fecha y hora

**Python 3.14 · Biblioteca estándar**

Accede a los componentes de `date` y `datetime` mediante atributos y obtén el día de la semana con `weekday()`.

## Atributos principales

```python
from datetime import datetime

fecha = datetime(2026, 10, 24, 9, 30, 45)

print(fecha.year)    # 2026
print(fecha.month)   # 10
print(fecha.day)     # 24
print(fecha.hour)    # 9
print(fecha.minute)  # 30
print(fecha.second)  # 45
```

- `year`, `month`, `day`: disponibles en `date` y `datetime`.
    
- `hour`, `minute`, `second`: disponibles en `datetime` y `time`.
    

## Obtener el día de la semana

```python
print(fecha.weekday())      # 5 = sábado
print(fecha.isoweekday())   # 6 = sábado
```

- `weekday()`: devuelve `0` para lunes y `6` para domingo.
    
- `isoweekday()`: devuelve `1` para lunes y `7` para domingo.
    

## Reglas de uso

- Accede a los componentes mediante atributos, sin paréntesis.
    
- Llama a `weekday()` e `isoweekday()` como métodos.
    
- Valida el rango de los días al convertirlos en índices: `weekday()` devuelve valores de `0` a `6`.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html) `[datetime](https://docs.python.org/3.14/library/datetime.html)`.