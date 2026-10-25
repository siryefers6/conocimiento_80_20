# Fechas _naive_ y _aware_ — Conciencia de zona horaria

**Python 3.14 · Biblioteca estándar**

Distingue los objetos _naive_, que no contienen información de zona horaria, de los _aware_, que permiten identificar un instante temporal mediante su zona horaria o desplazamiento UTC.

## Diferencias principales

|Tipo|Zona horaria|Ejemplo|
|---|---|---|
|_Naive_|No especificada|`datetime.now()`|
|_Aware_|Explícita|`datetime.now(UTC)`|

## Crear objetos

```python
from datetime import UTC, datetime

naive = datetime.now()
aware = datetime.now(UTC)

print(naive.tzinfo)  # None
print(aware.tzinfo)  # UTC
```

Un objeto `datetime` es _aware_ si `tzinfo` no es `None` y `tzinfo.utcoffset(objeto)` devuelve un desplazamiento distinto de `None`.

## Comprobar si una fecha es _aware_

```python
from datetime import datetime

def es_aware(fecha: datetime) -> bool:
    return (
        fecha.tzinfo is not None
        and fecha.utcoffset() is not None
    )
```

## Reglas importantes

- Usa fechas _aware_ para registros, eventos distribuidos y operaciones entre zonas horarias.
    
- Usa `datetime.now(UTC)` para registrar instantes en UTC.
    
- No mezcles objetos _naive_ y _aware_ al comparar fechas o calcular diferencias: puede producirse `TypeError`.
    
- No interpretes una fecha _naive_ como UTC automáticamente; su zona horaria es desconocida.
    
- Distingue el desplazamiento UTC de una zona geográfica: `-03:00` no contiene las reglas históricas ni los cambios estacionales de una región.
    

**Referencia:** [Documentación oficial: objetos](https://docs.python.org/3.14/library/datetime.html#aware-and-naive-objects) _[aware](https://docs.python.org/3.14/library/datetime.html#aware-and-naive-objects)_ [y](https://docs.python.org/3.14/library/datetime.html#aware-and-naive-objects) _[naive](https://docs.python.org/3.14/library/datetime.html#aware-and-naive-objects)_.