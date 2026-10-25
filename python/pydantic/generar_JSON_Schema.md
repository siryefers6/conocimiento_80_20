# Pydantic: generar JSON Schema

Utiliza `model_json_schema()` para generar un esquema JSON que describa los campos, tipos, restricciones y requisitos de un modelo.

## Ejemplo mínimo

```python
from pydantic import BaseModel, Field

class Producto(BaseModel):
    nombre: str = Field(min_length=3, max_length=50)
    precio: float = Field(gt=0)
    disponible: bool = True

esquema = Producto.model_json_schema()
print(esquema)
```

## Exporta el esquema a JSON

```python
import json

texto_json = json.dumps(
    Producto.model_json_schema(),
    indent=2,
)
```

## Genera un esquema para un contrato de API

```python
esquema = Producto.model_json_schema()

print(esquema["properties"])
print(esquema["required"])
```

- `properties`: describe los campos del modelo.
    
- `required`: identifica los campos obligatorios.
    
- `title` y `description`: documentan el modelo y sus campos cuando se definen.
    
- `minimum`, `exclusiveMinimum`, `minLength` y `maxLength`: representan restricciones compatibles con JSON Schema.
    
- `$defs` y `$ref`: permiten reutilizar definiciones, especialmente en modelos anidados.
    

## Modos de generación

```python
Producto.model_json_schema(mode="validation")
Producto.model_json_schema(mode="serialization")
```

- `validation`: representa el esquema esperado para validar entradas.
    
- `serialization`: representa la estructura de los datos serializados.
    

**Regla:** genera JSON Schema para documentar contratos, integrar herramientas y comprobar la estructura esperada. Recuerda que el esquema describe las reglas representables en JSON Schema; no sustituye toda la lógica de validación personalizada.

**Referencia:** Pydantic 2.14.1.