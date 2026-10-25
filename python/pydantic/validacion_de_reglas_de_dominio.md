# Pydantic: validación de reglas de dominio

Implementa reglas de negocio que dependan de las condiciones del dominio y no únicamente del tipo o formato de los datos.

## Ejemplo: validar un pedido

```python
from pydantic import BaseModel, model_validator

class Pedido(BaseModel):
    cantidad: int
    stock_disponible: int

    @model_validator(mode="after")
    def validar_stock(self):
        if self.cantidad > self.stock_disponible:
            raise ValueError("Stock insuficiente")
        return self
```

Pydantic comprueba que ambos campos sean enteros y que la cantidad no supere el stock declarado.

## Separa las responsabilidades

- **Pydantic:** valida tipos, formatos, rangos y coherencia entre campos.
    
- **Capa de servicio:** comprueba reglas que requieren consultar la base de datos o coordinar operaciones.
    
- **Base de datos:** garantiza restricciones de integridad, como claves únicas y relaciones.
    

## Ejemplo: comprobar disponibilidad real

```python
def crear_pedido(datos: PedidoCreate, servicio):
    producto = servicio.obtener_producto(datos.producto_id)

    if producto.stock < datos.cantidad:
        raise ValueError("Stock insuficiente")

    return servicio.registrar_pedido(datos)
```

No confíes en un valor de stock enviado por el cliente. Consulta el estado real en el servidor y protege las operaciones concurrentes mediante transacciones o mecanismos de control de concurrencia.

**Regla:** valida la estructura con Pydantic y aplica las reglas que dependen del estado del sistema en la capa de servicio.