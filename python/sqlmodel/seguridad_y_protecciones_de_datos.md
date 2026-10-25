# SQLModel 0.0.48: seguridad y protección de datos

## 1. Separa modelos de tabla y modelos de salida

No devuelvas directamente modelos de base de datos que contengan contraseñas, tokens o campos internos.

```python
from sqlmodel import SQLModel, Field

class Usuario(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    correo: str
    password_hash: str
    es_admin: bool = False

class UsuarioPublico(SQLModel):
    id: int
    correo: str
```

Declara el modelo público con una lista explícita de campos permitidos.

## 2. Controla la respuesta de FastAPI

Usa `response_model` para filtrar los campos que se pueden devolver.

```python
from fastapi import FastAPI
from sqlmodel import Session, select

app = FastAPI()

@app.get("/usuarios/{usuario_id}", response_model=UsuarioPublico)
def obtener_usuario(usuario_id: int, session: Session):
    return session.exec(
        select(Usuario).where(Usuario.id == usuario_id)
    ).first()
```

En una aplicación real, inyecta la sesión mediante `Depends(get_session)` y gestiona el caso de usuario inexistente.

## 3. Separa entrada, actualización y salida

Usa modelos diferentes para cada operación:

- `UsuarioCreate`: campos que el cliente puede enviar al crear.
    
- `UsuarioUpdate`: campos modificables, normalmente opcionales.
    
- `UsuarioPublico`: campos que el cliente puede consultar.
    
- `Usuario` (`table=True`): representación persistente.
    

No permitas que el cliente establezca directamente `es_admin`, `password_hash` o identificadores internos protegidos.

## 4. Protege credenciales y secretos

- Nunca almacenes contraseñas en texto plano; utiliza un algoritmo especializado de hashing de contraseñas.
    
- Guarda claves API y credenciales en variables de entorno o un gestor de secretos.
    
- Usa `pydantic-settings` para cargar la configuración desde el entorno.
    
- No incluyas secretos en respuestas JSON, mensajes de error, registros ni repositorios Git.
    
- No confundas ocultar un campo en la salida con protegerlo en la base de datos.
    

## 5. Controla los campos sensibles

- No uses `model_dump()` indiscriminadamente para construir respuestas públicas.
    
- No dependas exclusivamente de `Field(exclude=True)` para garantizar la seguridad de una API.
    
- Evita devolver trazas, consultas SQL con parámetros sensibles o detalles internos de excepciones.
    
- Aplica autorización en el servidor: filtrar campos no impide que un usuario consulte registros ajenos.
    

## 6. Prueba que los secretos no se filtren

```python
def test_usuario_publico_no_expone_secretos(client):
    response = client.get("/usuarios/1")

    assert response.status_code == 200
    data = response.json()

    assert "password_hash" not in data
    assert "es_admin" not in data
```

Adapta la prueba al comportamiento de tu endpoint y verifica también las respuestas de creación, actualización y error.

**Orden recomendado:** modelos de salida explícitos → `response_model` → modelos de entrada separados → gestión de secretos → autorización y pruebas.

**Prioriza:** aplica el principio de mínimo privilegio. Cada endpoint debe devolver únicamente los campos necesarios para su propósito.

**Referencias:**

- [FastAPI: modelos de respuesta](https://fastapi.tiangolo.com/tutorial/response-model/)
    
- [SQLModel: múltiples modelos para una tabla](https://sqlmodel.tiangolo.com/tutorial/fastapi/multiple-models/)
    
- [Pydantic: configuración y serialización](https://docs.pydantic.dev/latest/concepts/serialization/)
    
- [OWASP: API Security](https://owasp.org/www-project-api-security/)