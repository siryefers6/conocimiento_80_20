# Intervalos y vencimientos — Cálculo de plazos con `timedelta`

**Python 3.14 · Biblioteca estándar**

Utiliza `timedelta` para calcular fechas límite, antigüedad de registros y tiempos de espera a partir de fechas y horas.

## Calcular fechas límite

```python
from datetime import UTC, datetime, timedelta

creado_en = datetime.now(UTC)
fecha_limite = creado_en + timedelta(days=30)

print(fecha_limite)
```

## Calcular antigüedad de registros

```python
from datetime import UTC, datetime, timedelta

creado_en = datetime.now(UTC) - timedelta(days=5)
antiguedad = datetime.now(UTC) - creado_en

print(antiguedad.days)  # 5
```

## Comprobar vencimientos

```python
from datetime import UTC, datetime, timedelta

fecha_limite = datetime.now(UTC) + timedelta(hours=2)

if datetime.now(UTC) >= fecha_limite:
    print("Plazo vencido")
else:
    print("Plazo vigente")
```

## Calcular tiempos de espera

```python
from datetime import UTC, datetime, timedelta

ultimo_registro = datetime.now(UTC) - timedelta(minutes=45)
intervalo_minimo = timedelta(hours=1)

transcurrido = datetime.now(UTC) - ultimo_registro
restante = max(intervalo_minimo - transcurrido, timedelta())

print(restante.total_seconds())
```

`max()` evita devolver una espera negativa cuando el intervalo mínimo ya se cumplió.

## Reglas importantes

- Usa `timedelta(days=30)` para un plazo de 30 días exactos, no para un mes calendario.
    
- Usa `total_seconds()` para obtener la duración completa en segundos; `.seconds` no incluye los días completos.
    
- Compara marcas de tiempo _aware_ en una misma referencia temporal, preferiblemente UTC.
    
- Calcula el tiempo actual una sola vez por operación cuando necesites resultados consistentes.
    
- Para vencimientos definidos por fecha civil, aplica la zona horaria y las reglas del calendario antes de determinar el instante límite.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html#timedelta-objects) `[timedelta](https://docs.python.org/3.14/library/datetime.html#timedelta-objects)`.