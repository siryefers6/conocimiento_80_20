# Pydantic: validación y conversión de datos

Comprueba cómo Pydantic valida los valores de entrada y convierte tipos compatibles. Distingue entre modo normal (_lax_) y modo estricto (_strict_).

## Modo normal (`lax`)

Permite conversiones cuando el valor es compatible con el tipo declarado.

```python
from pydantic import BaseModel

class Producto(BaseModel):
    cantidad: int

producto = Producto(cantidad="10")
print(producto.cantidad)  # 10 (int)
```

## Modo estricto (`strict`)

Rechaza valores que no cumplan el tipo esperado sin las conversiones habituales.

```python
from pydantic import BaseModel, ConfigDict, ValidationError

class Producto(BaseModel):
    model_config = ConfigDict(strict=True)
    cantidad: int

try:
    Producto(cantidad="10")
except ValidationError as error:
    print(error)
```

## Activar el modo estricto por campo

```python
from pydantic import BaseModel, Field

class Producto(BaseModel):
    cantidad: int = Field(strict=True)
```

- **Lax:** convierte entradas compatibles; úsalo cuando recibas datos externos que necesiten coerción.
    
- **Strict:** exige mayor correspondencia entre el tipo recibido y el declarado; úsalo cuando las conversiones puedan ocultar errores.
    
- `**ValidationError**`**:** informa los errores de validación.
    
- **Validación estricta:** puede comportarse de forma diferente con datos Python y datos JSON; consulta el modo de entrada antes de asumir que ambos se rechazan igual.
    

**Regla:** elige explícitamente entre conversión flexible y validación estricta según el contrato de datos.