# Validación de fechas — Capturar `ValueError`

**Python 3.14 · Biblioteca estándar**

Valida fechas y horas capturando `ValueError` cuando los componentes están fuera de rango o una cadena no representa una fecha válida.

## Validar al construir objetos

```python
from datetime import date

try:
    fecha = date(2026, 2, 31)
except ValueError:
    print("Fecha inválida")
```

## Validar cadenas con `strptime()`

```python
from datetime import datetime

try:
    fecha = datetime.strptime("31/02/2026", "%d/%m/%Y")
except ValueError:
    print("Formato o fecha inválida")
```

## Validar cadenas ISO 8601

```python
from datetime import date

try:
    fecha = date.fromisoformat("2026-02-31")
except ValueError:
    print("Fecha inválida")
```

## Reglas importantes

- Captura `ValueError` en los límites de entrada de datos.
    
- Valida el formato y la existencia real de la fecha; por ejemplo, febrero no siempre tiene 29 días.
    
- No valides fechas únicamente con expresiones regulares: comprueban patrones textuales, no todas las reglas del calendario.
    
- Separa la validación sintáctica de las reglas de negocio, como fechas futuras o períodos permitidos.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html) `[datetime](https://docs.python.org/3.14/library/datetime.html)`.