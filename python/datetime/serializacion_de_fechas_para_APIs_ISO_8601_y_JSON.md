# Serialización de fechas para APIs — ISO 8601 y JSON

**Python 3.14 · Biblioteca estándar · FastAPI**

Utiliza ISO 8601 para intercambiar fechas y horas entre FastAPI, JSON y clientes. Serializa los valores como cadenas y conserva la zona horaria cuando representen instantes.

## Serializar fechas a ISO 8601

```python
from datetime import UTC, datetime, date

ahora = datetime.now(UTC)
hoy = date.today()

print(ahora.isoformat())
# Ejemplo: 2026-10-25T01:30:00+00:00

print(hoy.isoformat())
# Ejemplo: 2026-10-24
```

## Serializar con JSON

El módulo `json` no convierte automáticamente objetos `date` y `datetime`. Convierte los valores explícitamente:

```python
import json
from datetime import UTC, datetime

datos = {"creado_en": datetime.now(UTC)}

json_texto = json.dumps(
    datos,
    default=lambda valor: valor.isoformat(),
)
```

Limita `default` a tipos que sepas serializar; no conviertas indiscriminadamente objetos desconocidos a cadenas.

## Usar fechas en FastAPI

Con Pydantic, declara los tipos directamente y deja que FastAPI gestione la validación y serialización:

```python
from datetime import UTC, datetime

from pydantic import BaseModel, Field


class Registro(BaseModel):
    creado_en: datetime = Field(
        default_factory=lambda: datetime.now(UTC)
    )
```

FastAPI serializa el modelo como JSON y representa `creado_en` como una cadena ISO 8601. Valida por separado que las fechas recibidas incluyan zona horaria si tu API exige instantes inequívocos.

## Reglas importantes

- Usa `datetime` para instantes y `date` para fechas civiles sin hora.
    
- Prefiere UTC para intercambiar marcas de tiempo entre sistemas.
    
- Conserva el desplazamiento, por ejemplo `+00:00`, o normaliza el instante a UTC.
    
- Acepta y valida el formato esperado en las entradas de la API.
    
- No confundas una cadena ISO 8601 con un objeto `datetime`: usa `datetime.fromisoformat()` para convertirla cuando sea necesario.
    

**Referencias:** [Python:](https://docs.python.org/3.14/library/datetime.html) `[datetime](https://docs.python.org/3.14/library/datetime.html)` · [FastAPI: tipos de datos](https://fastapi.tiangolo.com/tutorial/extra-data-types/) · [Pydantic: serialización](https://docs.pydantic.dev/latest/concepts/serialization/).