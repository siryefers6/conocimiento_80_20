# Marcas de tiempo en bases de datos — UTC y hora local

**Python 3.14 · Biblioteca estándar**

Almacena los instantes en UTC y conviértelos a la zona horaria del usuario únicamente al mostrar los datos.

## Guardar marcas de tiempo en UTC

```python
from datetime import UTC, datetime

creado_en = datetime.now(UTC)

# Guarda creado_en en la base de datos.
```

Usa objetos _aware_ para evitar ambigüedades al registrar eventos, auditorías, operaciones y modificaciones.

## Convertir para mostrar

```python
from datetime import UTC, datetime
from zoneinfo import ZoneInfo

creado_en = datetime.now(UTC)
hora_local = creado_en.astimezone(ZoneInfo("America/Santiago"))

print(hora_local.strftime("%d/%m/%Y %H:%M:%S"))
```

Conserva el instante original; modifica únicamente su representación horaria.

## Reglas para bases de datos

- **Almacena instantes en UTC:** facilita comparaciones, ordenación e integración entre sistemas.
    
- **Convierte al presentar:** utiliza `ZoneInfo` con la zona geográfica correspondiente.
    
- **Usa tipos compatibles:** configura la columna para conservar la información horaria cuando el motor lo permita; verifica el comportamiento del ORM y del controlador.
    
- **Distingue fechas civiles:** para cumpleaños, vencimientos por día o fechas sin hora, utiliza `date` cuando corresponda; no conviertas automáticamente todo a UTC.
    
- **Conserva la zona de negocio:** si una cita depende de una región, almacena también su identificador IANA cuando sea necesario reconstruir la hora local prevista.
    
- **No mezcles fechas** _**naive**_ **y** _**aware**_**:** normaliza los datos antes de compararlos.
    

## Ejemplo con SQLModel

```python
from datetime import UTC, datetime

from sqlmodel import Field, SQLModel


class Registro(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    creado_en: datetime = Field(
        default_factory=lambda: datetime.now(UTC)
    )
```

Verifica que la base de datos y el controlador preserven o reconstruyan correctamente la zona horaria; algunos tipos, como `TIMESTAMP` de SQLite, no la conservan automáticamente.

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/datetime.html) `[datetime](https://docs.python.org/3.14/library/datetime.html)` · [SQLModel](https://sqlmodel.tiangolo.com/).