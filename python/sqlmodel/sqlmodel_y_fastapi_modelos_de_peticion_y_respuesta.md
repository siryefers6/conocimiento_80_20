# SQLModel y FastAPI: modelos de petición y respuesta

**Versión:** SQLModel 0.0.48 · FastAPI · **Referencia:** 24-10-2026

## Concepto

Separa los modelos según su responsabilidad:

- `**Create**`**:** define los datos que el cliente debe enviar para crear un registro.
    
- **Modelo de tabla:** representa los datos almacenados en la base de datos.
    
- `**Read**`**:** define los datos que la API puede exponer.
    
- `**Update**`**:** permite modificar parcialmente un registro.
    

## 1. Define los modelos

```python
from sqlmodel import SQLModel, Field


class ProductoCreate(SQLModel):
    nombre: str
    precio: float = Field(gt=0)


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str
    precio: float


class ProductoRead(SQLModel):
    id: int
    nombre: str
    precio: float


class ProductoUpdate(SQLModel):
    nombre: str | None = None
    precio: float | None = Field(default=None, gt=0)
```

El cliente no envía el identificador al crear el producto. El modelo de tabla lo almacena y el modelo de lectura lo expone.

## 2. Usa `response_model`

```python
from typing import Annotated
from fastapi import Depends, FastAPI, HTTPException
from sqlmodel import Session

app = FastAPI()

SessionDep = Annotated[Session, Depends(get_session)]


@app.post("/productos", response_model=ProductoRead, status_code=201)
def crear_producto(
    datos: ProductoCreate,
    session: SessionDep,
):
    producto = Producto.model_validate(datos)

    session.add(producto)
    session.commit()
    session.refresh(producto)

    return producto


@app.get("/productos/{producto_id}", response_model=ProductoRead)
def obtener_producto(producto_id: int, session: SessionDep):
    producto = session.get(Producto, producto_id)

    if producto is None:
        raise HTTPException(status_code=404, detail="Producto no encontrado")

    return producto
```

`response_model=ProductoRead` valida y filtra la respuesta según el modelo público.

## 3. Actualiza solo los campos enviados

```python
@app.patch("/productos/{producto_id}", response_model=ProductoRead)
def actualizar_producto(
    producto_id: int,
    datos: ProductoUpdate,
    session: SessionDep,
):
    producto = session.get(Producto, producto_id)

    if producto is None:
        raise HTTPException(status_code=404, detail="Producto no encontrado")

    cambios = datos.model_dump(exclude_unset=True)
    producto.sqlmodel_update(cambios)

    session.add(producto)
    session.commit()
    session.refresh(producto)

    return producto
```

Para actualizaciones parciales, `exclude_unset=True` evita sobrescribir campos que el cliente omitió.

## Reglas prácticas

- No recibas directamente el modelo de tabla en los endpoints de creación.
    
- Usa `response_model` para controlar los campos expuestos.
    
- Mantén los campos generados por la base de datos fuera del modelo de creación.
    
- Usa modelos específicos para cada operación; no expongas campos internos por comodidad.
    
- Coloca las reglas de negocio en la capa de servicio cuando deban reutilizarse entre endpoints web y JSON.