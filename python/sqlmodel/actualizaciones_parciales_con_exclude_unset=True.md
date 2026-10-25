# SQLModel: actualizaciones parciales con `exclude_unset=True`

**Versión:** SQLModel 0.0.48 · Pydantic 2.x · **Referencia:** 24-10-2026

## Concepto

Actualiza únicamente los campos enviados por el cliente, conservando los valores existentes de los campos omitidos.

## 1. Define un modelo de actualización

```python
from sqlmodel import SQLModel


class ProductoUpdate(SQLModel):
    nombre: str | None = None
    precio: float | None = None
    stock: int | None = None
```

Los valores predeterminados permiten omitir campos durante una actualización parcial.

## 2. Extrae únicamente los campos enviados

```python
datos = ProductoUpdate(nombre="Teclado")

cambios = datos.model_dump(exclude_unset=True)

# {"nombre": "Teclado"}
```

- `exclude_unset=True`: excluye los campos que no se enviaron.
    
- `exclude_none=True`: excluye los campos cuyo valor es `None`, incluso si fueron enviados.
    
- `exclude_defaults=True`: excluye los valores iguales a sus valores predeterminados.
    

Para actualizaciones parciales, prioriza `exclude_unset=True`.

## 3. Actualiza el registro

```python
from sqlmodel import Session


def actualizar_producto(
    session: Session,
    producto_id: int,
    datos: ProductoUpdate,
) -> Producto | None:
    producto = session.get(Producto, producto_id)

    if producto is None:
        return None

    cambios = datos.model_dump(exclude_unset=True)
    producto.sqlmodel_update(cambios)

    session.add(producto)
    session.commit()
    session.refresh(producto)

    return producto
```

Importa o define `Producto` según tu modelo de tabla.

## Reglas prácticas

- Usa un modelo independiente para actualizaciones parciales.
    
- Usa `model_dump(exclude_unset=True)` para distinguir campos omitidos de campos enviados explícitamente como `None`.
    
- No uses `exclude_none=True` si necesitas permitir que el cliente borre un valor nullable enviando `null`.
    
- Valida las restricciones de negocio antes de persistir.
    
- Devuelve `404 Not Found` desde el endpoint cuando el registro no exista.