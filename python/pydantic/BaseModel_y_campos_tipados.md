# Pydantic: `BaseModel` y campos tipados

Define modelos con `BaseModel` y anotaciones de tipos para validar datos y establecer estructuras explícitas.

## Tipos básicos

```python
from pydantic import BaseModel

class Producto(BaseModel):
    nombre: str
    cantidad: int
    precio: float
    disponible: bool
```

## Colecciones y modelos anidados

```python
class Direccion(BaseModel):
    ciudad: str
    codigo_postal: int

class Usuario(BaseModel):
    nombre: str
    etiquetas: list[str]
    metadatos: dict[str, str]
    direccion: Direccion
```

```python
usuario = Usuario(
    nombre="Ana",
    etiquetas=["admin", "activo"],
    metadatos={"origen": "web"},
    direccion={"ciudad": "Santiago", "codigo_postal": 8320000},
)
```

- `str`: texto.
    
- `int`: números enteros.
    
- `float`: números decimales.
    
- `bool`: valores booleanos.
    
- `list[T]`: lista de elementos del tipo `T`.
    
- `dict[K, V]`: diccionario con claves `K` y valores `V`.
    
- `Modelo`: valida estructuras anidadas y acepta diccionarios compatibles.
    

## Accede y serializa los datos

```python
print(usuario.direccion.ciudad)
print(usuario.model_dump())
```

**Regla:** utiliza `BaseModel` para validar entradas, representar entidades y definir contratos de API. Recuerda que Pydantic puede convertir tipos compatibles; las anotaciones no implican validación estricta por defecto.