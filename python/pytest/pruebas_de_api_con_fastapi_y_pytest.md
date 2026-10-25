# Pruebas de API con FastAPI y Pytest

Comprueba endpoints mediante solicitudes HTTP de prueba, validando códigos de estado, contenido JSON, validaciones y comportamiento de la API sin iniciar un servidor real.

## 1. `TestClient`

Utiliza `TestClient` de Starlette para pruebas síncronas. Instala las dependencias de prueba:

```sh
uv add --dev pytest httpx
```

```python
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

def test_obtener_pedido():
    response = client.get("/api/pedidos/1")

    assert response.status_code == 200
    assert response.json()["id"] == 1
```

Adapta la ruta y los datos al contrato real de tu API.

## 2. Verificar errores HTTP

```python
def test_pedido_inexistente():
    response = client.get("/api/pedidos/999999")

    assert response.status_code == 404
    assert response.json()["detail"] == "Pedido no encontrado"
```

Comprueba también respuestas `201`, `204`, `400`, `401`, `403`, `422` o `500` cuando correspondan al contrato de tu API.

## 3. Verificar validaciones

```python
def test_crear_pedido_sin_datos():
    response = client.post("/api/pedidos", json={})

    assert response.status_code == 422
```

## 4. HTTPX asíncrono

Usa `httpx.AsyncClient` con `ASGITransport` cuando necesites probar endpoints asíncronos:

```python
import pytest
from httpx import ASGITransport, AsyncClient
from app.main import app

@pytest.mark.anyio
async def test_endpoint():
    async with AsyncClient(
        transport=ASGITransport(app=app),
        base_url="http://test",
    ) as client:
        response = await client.get("/")

    assert response.status_code == 200
```

## Reglas

- Comprueba el código HTTP y el contenido de la respuesta.
    
- Sustituye dependencias con `app.dependency_overrides` cuando necesites una base de prueba.
    
- Limpia las sustituciones después de cada prueba.
    
- Usa `TestClient` para la mayoría de pruebas síncronas y HTTPX asíncrono cuando sea necesario.
    
- No dependas de una base de datos de producción.
    

**Prioriza:** `TestClient`, `status_code`, `response.json()` y la sustitución de dependencias para probar endpoints de forma aislada.