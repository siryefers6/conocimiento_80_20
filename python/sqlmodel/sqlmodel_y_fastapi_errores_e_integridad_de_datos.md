# FastAPI y SQLModel: errores e integridad de datos

**Versión:** SQLModel 0.0.48 · FastAPI · SQLAlchemy 2.x · **Referencia:** 24-10-2026

## 1. Traduce errores a códigos HTTP

| Situación                               | HTTP                        |
| --------------------------------------- | --------------------------- |
| Datos de entrada inválidos              | `422 Unprocessable Entity`  |
| Registro inexistente                    | `404 Not Found`             |
| Conflicto con una restricción única     | `409 Conflict`              |
| Clave foránea o integridad incompatible | `409 Conflict`              |
| Error inesperado del servidor           | `500 Internal Server Error` |

FastAPI valida automáticamente los modelos de entrada. Para errores de persistencia, captura las excepciones de SQLAlchemy y evita exponer detalles internos de la base de datos.

## 2. Maneja errores de integridad

```python
from fastapi import HTTPException
from sqlalchemy.exc import IntegrityError
from sqlmodel import Session


def guardar_producto(session: Session, producto: Producto) -> None:
    try:
        session.add(producto)
        session.commit()
    except IntegrityError as exc:
        session.rollback()
        raise HTTPException(
            status_code=409,
            detail="El registro entra en conflicto con los datos existentes",
        ) from exc
```

`IntegrityError` puede representar claves duplicadas, claves foráneas inválidas u otras restricciones. No asumas que siempre significa un duplicado: inspecciona el error específico del motor si necesitas mensajes diferentes.

## 3. Comprueba si el registro existe

```python
from fastapi import HTTPException
from sqlmodel import Session


def obtener_producto(session: Session, producto_id: int) -> Producto:
    producto = session.get(Producto, producto_id)

    if producto is None:
        raise HTTPException(
            status_code=404,
            detail="Producto no encontrado",
        )

    return producto
```

Reutiliza esta comprobación en operaciones de lectura, actualización y eliminación.

## 4. Maneja fallos inesperados de persistencia

```python
from fastapi import HTTPException
from sqlalchemy.exc import SQLAlchemyError


try:
    session.add(producto)
    session.commit()
except SQLAlchemyError as exc:
    session.rollback()
    logger.exception("Error de persistencia")
    raise HTTPException(
        status_code=500,
        detail="No se pudo completar la operación",
    ) from exc
```

Define previamente `logger` con el módulo `logging`. Este bloque es una alternativa general: si capturas `IntegrityError` por separado, colócalo antes de `SQLAlchemyError`.

## Reglas prácticas

- Ejecuta `session.rollback()` cuando falle una transacción antes de reutilizar la sesión.
    
- Captura `IntegrityError` para conflictos de integridad y `SQLAlchemyError` para otros errores de persistencia.
    
- Devuelve `409` para conflictos de integridad conocidos; reserva `500` para fallos inesperados.
    
- No devuelvas consultas SQL, trazas, credenciales ni mensajes internos de la base de datos al cliente.
    
- No conviertas cualquier excepción en `404` o `409`: clasifica el error según su causa.
    
- Usa restricciones `UNIQUE`, claves foráneas y `NOT NULL` en la base de datos para proteger la integridad incluso ante solicitudes concurrentes.
    
- Registra la excepción completa en los logs del servidor y devuelve al cliente un mensaje controlado.
    

## Prioriza

1. Gestiona registros inexistentes con `404`.
    
2. Gestiona conflictos de integridad con `409` y rollback.
    
3. Controla los errores inesperados con logs y respuestas `500` sin detalles internos.
    
4. Prueba cada caso con `pytest` y `TestClient`.