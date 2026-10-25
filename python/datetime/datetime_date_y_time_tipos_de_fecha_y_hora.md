# `datetime`, `date` y `time` — Tipos de fecha y hora

**Python 3.14 · Biblioteca estándar**

Importa los tipos desde `datetime` para representar fechas y horas sin depender de librerías externas.

## Tipos principales

|Tipo|Representación|Ejemplo|
|---|---|---|
|`datetime`|Fecha y hora|`2026-10-24 09:30:00`|
|`date`|Solo fecha|`2026-10-24`|
|`time`|Solo hora|`09:30:00`|

## Uso básico

```python
from datetime import date, datetime, time

fecha_hora = datetime(2026, 10, 24, 9, 30)
fecha = date(2026, 10, 24)
hora = time(9, 30)

print(fecha_hora.date())  # 2026-10-24
print(fecha_hora.time())  # 09:30:00
```

## Obtener valores actuales

```python
ahora = datetime.now()
hoy = date.today()

print(ahora)
print(hoy)
```

## Operaciones habituales

```python
fecha = date(2026, 10, 24)

print(fecha.year)   # 2026
print(fecha.month)  # 10
print(fecha.day)    # 24
```

Accede a los componentes mediante atributos como `year`, `month`, `day`, `hour`, `minute` y `second`, según el tipo.

## Reglas importantes

- Usa `date` para vencimientos, días y fechas de registro.
    
- Usa `time` para horarios sin fecha asociada.
    
- Usa `datetime` para eventos con fecha y hora.
    
- Usa `datetime.now()` para obtener la hora local actual.
    
- Usa `datetime.now().astimezone()` cuando necesites una fecha y hora local con zona horaria explícita.
    
- No confundas una fecha y hora sin zona horaria (_naive_) con una que incluye información de zona horaria (_aware_).
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html) `[datetime](https://docs.python.org/3.14/library/datetime.html)`.