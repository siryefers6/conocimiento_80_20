# SQLModel y FastAPI: sesiones con `Depends()`

**Versión:** SQLModel 0.0.48 · FastAPI · **Referencia:** 24-10-2026

## Concepto

Usa `Depends()` para proporcionar una sesión de base de datos a cada solicitud y cerrar la sesión al finalizar. Centraliza el ciclo de vida en una dependencia reutilizable.

## 1. Crea el motor y la dependencia

```python
from collections.abc import Generator

from fastapi import Depends, FastAPI
from sqlmodel import Session, SQLModel, create_engine

engine = create_engine("sqlite:///app.db")


def get_session() -> Generator[Session, None, None]:
    with Session(engine) as session:
        yield session


SessionDep = Depends(get_session)

app = FastAPI()
```

`yield` entrega la sesión al endpoint y el administrador de contexto la cierra al terminar.

## 2. Inyecta la sesión en un endpoint

```python
from typing import Annotated
from fastapi import Depends, HTTPException
from sqlmodel import Session

SessionDep = Annotated[Session, Depends(get_session)]


@app.get("/productos/{producto_id}")
def obtener_producto(
    producto_id: int,
    session: SessionDep,
):
    producto = session.get(Producto, producto_id)

    if producto is None:
        raise HTTPException(status_code=404, detail="Producto no encontrado")

    return producto
```

FastAPI ejecuta `get_session()` e inyecta el resultado en `session`. No crees manualmente una sesión dentro de cada endpoint.

## 3. Controla las escrituras

```python
@app.post("/productos", response_model=Producto)
def crear_producto(
    datos: ProductoCreate,
    session: SessionDep,
):
    producto = Producto.model_validate(datos)

    session.add(producto)
    session.commit()
    session.refresh(producto)

    return producto
```

- `add()`: prepara la inserción.
    
- `commit()`: confirma la transacción.
    
- `refresh()`: recupera valores generados o actualizados por la base de datos.
    

Si una operación falla durante una transacción, usa `session.rollback()` antes de reutilizar la sesión.

## Reglas prácticas

- Crea una sesión por solicitud; no compartas una misma sesión entre solicitudes concurrentes.
    
- Usa `Depends(get_session)` para reutilizar la dependencia.
    
- Cierra la sesión con `with Session(engine)` y `yield`.
    
- Mantén las transacciones de escritura explícitas en el servicio o endpoint.
    
- No confundas el cierre de sesión con `commit()`: cerrar no significa confirmar una transacción pendiente.
    
- Para mantener una arquitectura sencilla, reutiliza la misma dependencia en endpoints web y JSON.