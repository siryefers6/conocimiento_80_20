# FastAPI: separación entre routers, servicios y persistencia

**Versión:** FastAPI · SQLModel 0.0.48 · **Referencia:** 24-10-2026

## Concepto

Separa responsabilidades en tres capas para mantener el código reutilizable, comprobable y fácil de modificar.

## 1. Router: gestiona HTTP

Recibe solicitudes, valida entradas y define respuestas HTTP. No implementa reglas de negocio ni consultas SQL.

```python
# routers/productos.py
from fastapi import APIRouter, Depends
from sqlmodel import Session
from typing import Annotated

router = APIRouter(prefix="/productos", tags=["productos"])
SessionDep = Annotated[Session, Depends(get_session)]


@router.post("/")
def crear(datos: ProductoCreate, session: SessionDep):
    return crear_producto(session, datos)
```

## 2. Servicio: aplica reglas de negocio

Coordina operaciones, valida condiciones de negocio y decide qué acciones ejecutar. No depende de `APIRouter` ni de `HTTPException`.

```python
# services/productos.py
from sqlmodel import Session


def crear_producto(session: Session, datos: ProductoCreate) -> Producto:
    producto = Producto.model_validate(datos)
    return guardar_producto(session, producto)
```

## 3. Persistencia: accede a la base de datos

Centraliza las consultas y operaciones de almacenamiento.

```python
# repositories/productos.py
from sqlmodel import Session


def guardar_producto(session: Session, producto: Producto) -> Producto:
    session.add(producto)
    session.commit()
    session.refresh(producto)
    return producto
```

## Flujo de una solicitud

```
Cliente HTTP
    ↓
Router → valida entrada y traduce errores HTTP
    ↓
Servicio → aplica reglas de negocio
    ↓
Repositorio → consulta o modifica la base de datos
    ↓
Base de datos
```

## Reglas prácticas

- **Router:** HTTP, códigos de estado, parámetros y modelos de petición/respuesta.
    
- **Servicio:** reglas de negocio y coordinación de operaciones.
    
- **Repositorio:** consultas SQLModel y persistencia.
    
- **Sesión:** inyéctala mediante `Depends()` y pásala a las capas que la necesiten.
    
- **Transacciones:** define claramente qué capa controla `commit()` y `rollback()`; evita confirmaciones dispersas entre varias capas.
    
- **Errores:** utiliza excepciones propias o de dominio en los servicios y tradúcelas a HTTP en el router.
    
- **Simplicidad:** no crees abstracciones innecesarias para operaciones CRUD pequeñas; separa las capas cuando sus responsabilidades lo justifiquen.
    

## Estructura recomendada

```
app/
├── main.py
├── database.py
├── routers/
│   └── productos.py
├── services/
│   └── productos.py
├── repositories/
│   └── productos.py
└── models/
    └── productos.py
```

**Prioriza:** mantén HTTP en los routers, las reglas de negocio en los servicios y el acceso a datos en la persistencia. Así podrás reutilizar los servicios desde endpoints web, API JSON y pruebas automatizadas.