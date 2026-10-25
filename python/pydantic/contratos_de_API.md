# Pydantic y FastAPI: contratos de API

Define modelos Pydantic para validar solicitudes, estructurar respuestas y documentar automáticamente los contratos de una API.

## 1. Valida solicitudes

Utiliza `BaseModel` para declarar el cuerpo esperado de una petición.

```python
from pydantic import BaseModel, Field
from fastapi import FastAPI

app = FastAPI()

class ProductoCreate(BaseModel):
    nombre: str = Field(min_length=3, max_length=100)
    precio: float = Field(gt=0)

@app.post("/productos")
def crear_producto(producto: ProductoCreate):
    return producto
```

FastAPI valida el JSON recibido antes de ejecutar el endpoint y devuelve un error `422` si los datos incumplen el contrato.

## 2. Valida respuestas

Utiliza `response_model` para validar y filtrar la salida.

```python
class ProductoResponse(BaseModel):
    id: int
    nombre: str
    precio: float

@app.get("/productos/{producto_id}", response_model=ProductoResponse)
def obtener_producto(producto_id: int):
    return {
        "id": producto_id,
        "nombre": "Teclado",
        "precio": 19990,
        "costo_interno": 12000,
    }
```

FastAPI excluye `costo_interno` porque no forma parte de `ProductoResponse`.

## 3. Separa los contratos

- `ProductoCreate`: datos que acepta la API.
    
- `ProductoResponse`: datos que devuelve la API.
    
- `ProductoUpdate`: campos modificables, normalmente opcionales.
    

No reutilices un único modelo para todas las operaciones si sus requisitos son diferentes.

## Reglas prácticas

- Usa `Field` para restricciones de entrada.
    
- Declara `response_model` explícitamente cuando necesites controlar la salida.
    
- No expongas contraseñas, claves ni campos internos.
    
- Aprovecha los esquemas generados para documentar la API mediante OpenAPI.
    

**Regla:** valida las entradas con modelos Pydantic y define explícitamente qué datos puede recibir y devolver cada endpoint.