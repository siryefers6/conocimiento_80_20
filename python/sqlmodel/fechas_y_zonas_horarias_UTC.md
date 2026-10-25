# SQLModel 0.0.48: fechas y zonas horarias UTC

**Referencia:** SQLModel 0.0.45+ · Python 3.11+ · 24-10-2026

## 1. Cambio introducido en SQLModel 0.0.45

Desde la versión `0.0.45`, los campos `datetime` de SQLModel utilizan `UTCDateTime` por defecto.

- Requieren fechas con información de zona horaria al interactuar con la base de datos.
    
- Normalizan los valores a UTC al escribirlos.
    
- Devuelven fechas conscientes de zona horaria (`aware`) en UTC al leerlas.
    
- El comportamiento funciona también con SQLite, aunque su almacenamiento no conserve la zona horaria.
    

Este cambio es incompatible con código que utiliza `datetime.now()` sin zona horaria. [https://sqlmodel.tiangolo.com/advanced/datetime/](https://sqlmodel.tiangolo.com/advanced/datetime/)

## 2. Genera fechas UTC correctamente

```python
from datetime import UTC, datetime
from sqlmodel import Field, SQLModel

class Evento(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    creado_en: datetime = Field(
        default_factory=lambda: datetime.now(UTC)
    )
```

Usa `default_factory` para calcular la fecha al crear cada instancia, no al definir la clase.

Para Python 3.10, utiliza `datetime.now(timezone.utc)`.

## 3. Diferencia entre fechas `aware` y `naive`

```python
from datetime import UTC, datetime

# Correcto: incluye zona horaria
fecha_utc = datetime.now(UTC)

# Incorrecto para parámetros de base de datos UTCDateTime
fecha_naive = datetime.now()
```

Una fecha `aware` identifica un instante mediante su zona horaria. Una fecha `naive` no indica a qué zona pertenece.

No interpretes automáticamente una fecha `naive` como UTC: podría representar la hora local de otro sistema.

## 4. Usa `AwareDatetime` y `NaiveDatetime`

Cuando necesites expresar explícitamente el tipo de fecha esperado:

```python
from pydantic import AwareDatetime, NaiveDatetime
from sqlmodel import SQLModel

class Registro(SQLModel):
    instante: AwareDatetime
    hora_local: NaiveDatetime
```

- `AwareDatetime`: exige información de zona horaria durante la validación Pydantic.
    
- `NaiveDatetime`: representa una fecha sin zona horaria y selecciona `DateTime(timezone=False)` en SQLModel.
    
- `datetime` normal sigue aceptando valores `aware` y `naive` durante la validación Pydantic, pero los parámetros `naive` fallan al interactuar con `UTCDateTime`.
    

Para modelos de tabla, usa `model_validate()` cuando necesites aplicar explícitamente la validación Pydantic; la construcción directa con `Modelo(...)` no garantiza todas esas validaciones.

## 5. Diferencias entre bases de datos

| Base de datos | Comportamiento                                                                                                   |
| ------------- | ---------------------------------------------------------------------------------------------------------------- |
| SQLite        | Almacena fechas normalmente sin zona horaria; SQLModel restaura UTC al leerlas.                                  |
| PostgreSQL    | Utiliza `TIMESTAMP WITH TIME ZONE`; conserva el instante, no el nombre ni el desplazamiento original de la zona. |
| MySQL/MariaDB | Utiliza normalmente `DATETIME`; SQLModel normaliza y restaura UTC mediante su tipo personalizado.                |

## 6. Consultas y comparaciones

Usa valores `aware` también en filtros, actualizaciones y comparaciones.

```python
from datetime import UTC, datetime
from sqlmodel import select

ahora = datetime.now(UTC)
statement = select(Evento).where(Evento.creado_en <= ahora)
```

Evita mezclar fechas `aware` y `naive` en operaciones de Python.

## 7. Actualiza aplicaciones existentes

El cambio de versión no modifica automáticamente las columnas ni convierte los datos almacenados.

1. Identifica qué zona horaria representan las fechas existentes.
    
2. Haz una copia de seguridad.
    
3. Convierte los datos a UTC cuando sea necesario.
    
4. Migra las columnas PostgreSQL a `TIMESTAMP WITH TIME ZONE`, indicando explícitamente la zona de los valores antiguos.
    
5. Actualiza los generadores de fechas, filtros, valores por defecto de la base de datos y otras aplicaciones que escriban en las mismas tablas.
    
6. Prueba las migraciones y las lecturas/escrituras con el mismo controlador utilizado en producción.
    

En SQLite y MySQL/MariaDB, la columna puede mantener su definición, pero los datos antiguos que no estén en UTC requieren una migración de datos. Si necesitas conservar el comportamiento `naive`, declara `NaiveDatetime` o usa `Field(sa_type=DateTime(timezone=False))`.

**Orden recomendado:** `datetime.now(UTC)` → fechas `aware`/`naive` → `AwareDatetime`/`NaiveDatetime` → consultas → migración de datos existentes.

**Prioriza:** utiliza UTC para representar instantes, conserva las zonas locales por separado cuando sean relevantes y nunca conviertas datos antiguos sin determinar primero qué zona horaria representan.

**Referencias:**

- [SQLModel: Datetimes and Timezones](https://sqlmodel.tiangolo.com/advanced/datetime/)
    
- [SQLModel 0.0.45: notas de versión](https://github.com/fastapi/sqlmodel/releases/tag/0.0.45)
    
- [Python: objetos datetime conscientes y sin zona horaria](https://docs.python.org/3/library/datetime.html#aware-and-naive-objects)