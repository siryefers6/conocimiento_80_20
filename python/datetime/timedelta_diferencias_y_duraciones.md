# `timedelta` — Diferencias y duraciones

**Python 3.14 · Biblioteca estándar**

Usa `timedelta` para representar duraciones y sumar o restar intervalos de tiempo a objetos `date` y `datetime`.

## Crear duraciones

```python
from datetime import timedelta

dias = timedelta(days=7)
horas = timedelta(hours=3)
intervalo = timedelta(days=2, hours=4, minutes=30)
```

Admite `weeks`, `days`, `hours`, `minutes`, `seconds`, `milliseconds` y `microseconds`.

## Sumar y restar tiempo

```python
from datetime import date, timedelta

hoy = date.today()

vencimiento = hoy + timedelta(days=30)
ayer = hoy - timedelta(days=1)

print(vencimiento)
print(ayer)
```

## Calcular diferencias

```python
from datetime import datetime

inicio = datetime(2026, 10, 24, 8, 0)
fin = datetime(2026, 10, 24, 10, 45)

duracion = fin - inicio

print(duracion)             # 2:45:00
print(duracion.total_seconds())  # 9900.0
```

La resta entre dos objetos `date` o dos objetos `datetime` compatibles devuelve un `timedelta`.

## Obtener componentes

```python
duracion = timedelta(days=2, hours=5)

print(duracion.days)          # 2
print(duracion.seconds)       # 18000
print(duracion.total_seconds())  # 190800.0
```

- `days`: días completos normalizados.
    
- `seconds`: segundos restantes dentro del día normalizado; no representa la duración total.
    
- `total_seconds()`: duración completa expresada en segundos.
    

## Reglas importantes

- Usa `timedelta` para calcular vencimientos, antigüedad, tiempos transcurridos e intervalos.
    
- Compara duraciones directamente: `duracion > timedelta(hours=1)`.
    
- No accedas a `.hour` o `.minute`: `timedelta` no proporciona esos atributos.
    
- Ten en cuenta que `timedelta` almacena duraciones con resolución de microsegundos, no meses ni años calendario.
    
- Comprueba la compatibilidad de zonas horarias al restar objetos `datetime`.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html#timedelta-objects) `[timedelta](https://docs.python.org/3.14/library/datetime.html#timedelta-objects)`.