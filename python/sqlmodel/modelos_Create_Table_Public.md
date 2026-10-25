# SQLModel: modelos Create, Table y Public

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Separa los modelos según su responsabilidad para controlar los datos recibidos, almacenados y expuestos por la API.

- **Create:** valida los datos de entrada.
    
- **Table:** representa la tabla y permite persistir registros.
    
- **Public:** define los datos que puede recibir el cliente como respuesta.
    

## Ejemplo funcional

```python
from sqlmodel import Field, SQLModel


class ProductoCreate(SQLModel):
    nombre: str
    precio: float


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str
    precio: float
    costo_interno: float


class ProductoPublic(SQLModel):
    id: int
    nombre: str
    precio: float
```

## Conversión entre modelos

```python
datos = ProductoCreate(nombre="Teclado", precio=25_000)

producto = Producto.model_validate(datos)
producto.costo_interno = 15_000

respuesta = ProductoPublic.model_validate(producto)
```

Utiliza `model_validate()` para construir modelos a partir de otros objetos compatibles y validar los datos resultantes.

## Responsabilidades

| Modelo           | Responsabilidad | Incluye                                   |
| ---------------- | --------------- | ----------------------------------------- |
| `ProductoCreate` | Entrada         | Campos que el cliente puede enviar        |
| `Producto`       | Persistencia    | Columnas, clave primaria y datos internos |
| `ProductoPublic` | Salida          | Solo los campos que se permite exponer    |

## Reglas prácticas

- No permitas que el cliente establezca libremente la clave primaria ni los campos internos.
    
- No devuelvas directamente el modelo de tabla si contiene información privada.
    
- Define modelos `Update` independientes cuando necesites actualizaciones parciales.
    
- Comprueba las restricciones de entrada con Pydantic y las restricciones de integridad con la base de datos.
    

**Prioriza:** separación de responsabilidades, conversión entre modelos y control de los datos expuestos por FastAPI.