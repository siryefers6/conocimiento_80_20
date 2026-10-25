# FastAPI y SQLModel: CRUD mediante endpoints

**Versión:** SQLModel 0.0.48 · FastAPI · **Referencia:** 24-10-2026

## Concepto

Implementa las cuatro operaciones CRUD mediante métodos HTTP:

- `POST`: crear un registro.
    
- `GET`: consultar uno o varios registros.
    
- `PATCH`: actualizar campos específicos.
    
- `DELETE`: eliminar un registro.
    

## 1. Define los modelos

```python
from sqlmodel import SQLModel, Field


class ProductoBase(SQLModel):
    nombre: str
    precio: float = Field(gt=0)


class Producto(ProductoBase, table=True):
    id: int | None = Field(default=None, primary_key=True)


class ProductoCreate(ProductoBase):
    pass


class ProductoRead(ProductoBase):
    id: int


class ProductoUpdate(SQLModel):
    nombre: str | None = None
    precio: float | None = Field(default=None, gt=0)
```

## 2. Implementa los endpoints

Reutiliza la dependencia `get_session()` definida en la nota sobre `Depends()`.

```python
from typing import Annotated

from fastapi import Depends, FastAPI, HTTPException, Response
from sqlmodel import Session, select

app = FastAPI()
SessionDep = Annotated[Session, Depends(get_session)]


@app.post("/productos", response_model=ProductoRead, status_code=201)
def crear_producto(datos: ProductoCreate, session: SessionDep):
    producto = Producto.model_validate(datos)
    session.add(producto)
    session.commit()
    session.refresh(producto)
    return producto


@app.get("/productos", response_model=list[ProductoRead])
def listar_productos(session: SessionDep):
    return session.exec(select(Producto).order_by(Producto.id)).all()


@app.get("/productos/{producto_id}", response_model=ProductoRead)
def obtener_producto(producto_id: int, session: SessionDep):
    producto = session.get(Producto, producto_id)

    if producto is None:
        raise HTTPException(404, "Producto no encontrado")

    return producto


@app.patch("/productos/{producto_id}", response_model=ProductoRead)
def actualizar_producto(
    producto_id: int,
    datos: ProductoUpdate,
    session: SessionDep,
):
    producto = session.get(Producto, producto_id)

    if producto is None:
        raise HTTPException(404, "Producto no encontrado")

    cambios = datos.model_dump(exclude_unset=True)
    producto.sqlmodel_update(cambios)

    session.add(producto)
    session.commit()
    session.refresh(producto)
    return producto


@app.delete("/productos/{producto_id}", status_code=204)
def eliminar_producto(producto_id: int, session: SessionDep):
    producto = session.get(Producto, producto_id)

    if producto is None:
        raise HTTPException(404, "Producto no encontrado")

    session.delete(producto)
    session.commit()
    return Response(status_code=204)
```

## Reglas prácticas

- Usa `response_model` para controlar los datos expuestos por la API.
    
- Devuelve `201 Created` al crear y `204 No Content` al eliminar sin cuerpo de respuesta.
    
- Devuelve `404 Not Found` cuando el registro solicitado no existe.
    
- Usa `exclude_unset=True` en `PATCH` para conservar los campos omitidos.
    
- Mantén las operaciones de base de datos dentro de transacciones explícitas.
    
- Separa las reglas de negocio en servicios cuando deban reutilizarse entre endpoints web y JSON.
    

## Orden recomendado

1. Implementa `POST` y `GET`.
    
2. Añade `PATCH` y comprueba las actualizaciones parciales.
    
3. Implementa `DELETE` y verifica los registros inexistentes.
    
4. Prueba los códigos HTTP y la persistencia con `pytest` y `TestClient`.