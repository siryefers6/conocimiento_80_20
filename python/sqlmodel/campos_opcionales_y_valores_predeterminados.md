# SQLModel: campos opcionales y valores predeterminados

**Versión:** SQLModel 0.0.48 · **Referencia:** 24-10-2026

## 1. Distingue los tres estados

- **Campo omitido:** el cliente no envía el campo.
    
- `**None**`**:** el campo recibe explícitamente un valor nulo, si el modelo lo permite.
    
- **Valor predeterminado:** se asigna un valor cuando el campo se omite.
    
- **Valor generado por la base de datos:** se asigna al insertar el registro, por ejemplo, mediante `DEFAULT`, `IDENTITY` o una columna autoincremental.
    

## 2. Campos opcionales y valores predeterminados

```python
from sqlmodel import SQLModel, Field


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str
    descripcion: str | None = None
    stock: int = 0
```

- `nombre`: obligatorio.
    
- `descripcion`: admite `None` y se establece en `None` si se omite.
    
- `stock`: usa `0` si se omite.
    
- `id`: se genera normalmente mediante autoincremento al insertar el registro.
    

**Importante:** `str | None` permite `None`, pero no hace que el campo sea opcional durante la creación si no tiene un valor predeterminado.

## 3. Distingue omisión y `None` en actualizaciones

```python
actualizacion = {
    "descripcion": None,
    "stock": 5,
}

datos = ProductoUpdate(**actualizacion)

cambios = datos.model_dump(exclude_unset=True)
```

`exclude_unset=True` conserva los campos enviados, incluido `descripcion=None`, y excluye los campos omitidos.

Para aplicar la actualización:

```python
producto.sqlmodel_update(cambios)
session.add(producto)
session.commit()
session.refresh(producto)
```

Usa un modelo de actualización con campos opcionales y valores predeterminados `None` cuando necesites permitir actualizaciones parciales.

## 4. Valores generados por la base de datos

```python
from datetime import datetime
from sqlalchemy import Column, DateTime, func
from sqlmodel import SQLModel, Field


class Registro(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)

    creado_en: datetime | None = Field(
        default=None,
        sa_column=Column(
            DateTime,
            server_default=func.current_timestamp(),
            nullable=False,
        ),
    )
```

`server_default` delega el valor predeterminado al motor de base de datos. Tras insertar, utiliza `session.refresh(registro)` si necesitas recuperar el valor generado y todavía no está disponible en el objeto.

## Reglas prácticas

- Usa `campo: str` para datos obligatorios.
    
- Usa `campo: str | None = None` para datos que admiten nulos y pueden omitirse.
    
- Usa `campo: int = 0` para un valor predeterminado definido por la aplicación.
    
- Usa `server_default` para valores predeterminados definidos por la base de datos.
    
- Usa `model_dump(exclude_unset=True)` para conservar la diferencia entre campo omitido y campo enviado como `None`.
    
- No confundas `default` —gestionado por SQLModel/SQLAlchemy— con `server_default` —gestionado por la base de datos—.