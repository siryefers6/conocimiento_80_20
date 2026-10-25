# FastAPI: separar modelos de entrada y salida

Define modelos Pydantic independientes para los datos recibidos, almacenados y devueltos. Evita exponer campos internos o permitir que el cliente modifique atributos protegidos.

## Ejemplo práctico

```python
from pydantic import BaseModel, Field
from fastapi import FastAPI

app = FastAPI()

class UsuarioCreate(BaseModel):
    nombre: str = Field(min_length=2)
    correo: str

class UsuarioResponse(BaseModel):
    id: int
    nombre: str
    correo: str

@app.post("/usuarios", response_model=UsuarioResponse)
def crear_usuario(datos: UsuarioCreate):
    return {
        "id": 1,
        "nombre": datos.nombre,
        "correo": datos.correo,
        "es_administrador": False,
    }
```

- `UsuarioCreate`: define los campos que el cliente puede enviar.
    
- `UsuarioResponse`: determina los campos que la API devuelve.
    
- `response_model`: valida y filtra la respuesta según el contrato declarado.
    

## Reglas prácticas

- Usa sufijos como `Create`, `Update` y `Response`.
    
- No incluyas `id` en el modelo de creación si lo genera el servidor.
    
- Excluye contraseñas y datos internos del modelo de respuesta.
    
- Define campos opcionales en `Update` cuando permitas modificaciones parciales.
    

**Regla:** separa los modelos de entrada y salida para controlar los permisos, la validación y la exposición de datos.