# Ordenación y filtrado temporal — Comparaciones y rangos

**Python 3.14 · Biblioteca estándar**

Ordena fechas cronológicamente y filtra registros mediante límites temporales inclusivos o exclusivos.

## Ordenar fechas

```python
from datetime import date

fechas = [
    date(2026, 10, 24),
    date(2026, 1, 15),
    date(2026, 6, 10),
]

fechas.sort()
print(fechas)
```

Usa `sorted(fechas)` para crear una lista ordenada sin modificar la original.

## Filtrar con límites inclusivos

Incluye los registros que coincidan con las fechas inicial y final.

```python
from datetime import date

inicio = date(2026, 10, 1)
fin = date(2026, 10, 31)

registros = [
    date(2026, 9, 30),
    date(2026, 10, 15),
    date(2026, 10, 31),
]

filtrados = [f for f in registros if inicio <= f <= fin]
```

## Filtrar con límites exclusivos

Excluye los registros que coincidan exactamente con cualquiera de los límites.

```python
filtrados = [f for f in registros if inicio < f < fin]
```

## Consultar intervalos con `datetime`

Para consultar un día completo, utiliza un intervalo semiabierto: incluye el inicio y excluye el inicio del día siguiente.

```python
from datetime import date, datetime, time, timedelta

dia = date(2026, 10, 24)
inicio = datetime.combine(dia, time.min)
fin = datetime.combine(dia + timedelta(days=1), time.min)

filtrados = [f for f in marcas_temporales if inicio <= f < fin]
```

Este patrón evita depender de la precisión de microsegundos para definir el final del día. Asegúrate de que `marcas_temporales` contenga objetos `datetime` compatibles con los límites.

## Reglas importantes

- Usa `sorted()` o `.sort()` para ordenar cronológicamente.
    
- Define explícitamente si los límites son inclusivos (`<=`, `>=`) o exclusivos (`<`, `>`).
    
- Prefiere intervalos semiabiertos `[inicio, fin)` para consultas temporales.
    
- No mezcles objetos `datetime` _naive_ y _aware_.
    
- Normaliza las marcas de tiempo a UTC cuando compares instantes almacenados en distintas zonas horarias.
    
- Para consultas por día local, calcula los límites en la zona geográfica correspondiente antes de convertirlos a UTC.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html) `[datetime](https://docs.python.org/3.14/library/datetime.html)`.