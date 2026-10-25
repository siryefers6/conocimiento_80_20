# Comparaciones y operaciones aritméticas con fechas

**Python 3.14 · Biblioteca estándar**

Compara objetos `date` y `datetime`, calcula diferencias con la resta y modifica fechas mediante `timedelta`.

## Comparar fechas y horas

```python
from datetime import date

fecha1 = date(2026, 10, 24)
fecha2 = date(2026, 10, 30)

print(fecha1 < fecha2)   # True
print(fecha1 == fecha2)  # False
print(fecha1 > fecha2)   # False
```

Usa `==`, `!=`, `<`, `<=`, `>` y `>=`. Compara objetos del mismo tipo para mantener la semántica clara.

## Calcular diferencias

```python
from datetime import date

inicio = date(2026, 10, 24)
fin = date(2026, 10, 30)

diferencia = fin - inicio

print(diferencia.days)  # 6
```

La resta entre dos objetos `date` devuelve un `timedelta`. Con `datetime`, calcula también la diferencia horaria.

## Sumar y restar duraciones

```python
from datetime import date, timedelta

hoy = date(2026, 10, 24)

futuro = hoy + timedelta(days=7)
pasado = hoy - timedelta(days=3)

print(futuro)  # 2026-10-31
print(pasado)  # 2026-10-21
```

## Reglas importantes

- Resta fechas compatibles para obtener un `timedelta`.
    
- Usa `timedelta` para sumar o restar días, semanas, horas o segundos.
    
- No sumes directamente dos objetos `date` ni dos objetos `datetime`.
    
- No mezcles `datetime` con zona horaria (_aware_) y `datetime` sin ella (_naive_) en comparaciones ordenadas o restas.
    
- Recuerda que un día de `timedelta` equivale a 24 horas, no necesariamente a un día de calendario local cuando intervienen cambios de horario.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html) `[datetime](https://docs.python.org/3.14/library/datetime.html)`.