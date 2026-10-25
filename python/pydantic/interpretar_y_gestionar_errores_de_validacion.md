# Pydantic: interpretar y gestionar errores de validación

Captura `ValidationError` y utiliza `errors()` para identificar los campos inválidos y generar mensajes seguros para el usuario.

## Inspecciona los errores

```python
from pydantic import BaseModel, ValidationError

class Usuario(BaseModel):
    nombre: str
    edad: int

try:
    Usuario(nombre="Ana", edad="abc")
except ValidationError as error:
    for detalle in error.errors():
        print(detalle["loc"], detalle["type"])
```

- `loc`: ubicación del campo que falló.
    
- `type`: código del error.
    
- `msg`: descripción del error.
    
- `input`: valor recibido; puede contener información sensible.
    
- `ctx`: contexto adicional que puede incluir detalles internos.
    

## Devuelve errores controlados

```python
try:
    usuario = Usuario(nombre="Ana", edad="abc")
except ValidationError as error:
    errores = [
        {
            "campo": ".".join(map(str, detalle["loc"])),
            "mensaje": "Valor inválido",
            "codigo": detalle["type"],
        }
        for detalle in error.errors()
    ]
```

## Buenas prácticas

- No devuelvas directamente `str(error)` en respuestas públicas.
    
- No expongas `input`, `ctx` ni trazas internas al cliente.
    
- Traduce los códigos técnicos a mensajes comprensibles.
    
- Conserva los detalles completos únicamente en registros protegidos cuando sean necesarios.
    
- En FastAPI, centraliza el tratamiento de errores de validación para mantener respuestas consistentes.
    

**Regla:** separa el diagnóstico técnico interno del mensaje público. Devuelve solo el campo, un mensaje seguro y, si resulta útil, un código de error estable.

**Referencia:** Pydantic 2.14.1.