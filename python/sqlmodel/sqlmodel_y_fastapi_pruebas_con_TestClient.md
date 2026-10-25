# FastAPI y SQLModel: pruebas con `TestClient`

**Versión:** SQLModel 0.0.48 · FastAPI · Pytest · **Referencia:** 24-10-2026

## 1. Concepto

Prueba los endpoints sin iniciar un servidor real. Usa `TestClient`, una base de datos SQLite aislada y `app.dependency_overrides` para sustituir la sesión de producción.

## 2. Configura una base de datos de prueba

```python
# tests/conftest.py
import pytest
from collections.abc import Generator

from fastapi.testclient import TestClient
from sqlmodel import SQLModel, Session, create_engine
from sqlalchemy.pool import StaticPool

from app.main import app
from app.database import get_session


@pytest.fixture
def client() -> Generator[TestClient, None, None]:
    engine = create_engine(
        "sqlite://",
        connect_args={"check_same_thread": False},
        poolclass=StaticPool,
    )
    SQLModel.metadata.create_all(engine)

    def get_test_session():
        with Session(engine) as session:
            yield session

    app.dependency_overrides[get_session] = get_test_session

    try:
        with TestClient(app) as test_client:
            yield test_client
    finally:
        app.dependency_overrides.clear()
        SQLModel.metadata.drop_all(engine)
        engine.dispose()
```

- `sqlite://` crea una base de datos SQLite en memoria.
    
- `StaticPool` permite compartir la misma conexión entre operaciones de prueba.
    
- `dependency_overrides` sustituye la dependencia original por una que utiliza el motor de prueba.
    
- El bloque `finally` restaura el estado y libera los recursos.
    

Asegúrate de importar todos los modelos antes de ejecutar `SQLModel.metadata.create_all()`.

## 3. Prueba el CRUD mediante HTTP

```python
# tests/test_productos.py

def test_crear_producto(client):
    respuesta = client.post(
        "/productos",
        json={"nombre": "Teclado", "precio": 25.0},
    )

    assert respuesta.status_code == 201
    assert respuesta.json()["nombre"] == "Teclado"
    assert respuesta.json()["id"] is not None


def test_producto_inexistente(client):
    respuesta = client.get("/productos/999")

    assert respuesta.status_code == 404
```

Ejecuta las pruebas:

```
uv run pytest -v
```

## 4. Qué debes comprobar

- **Creación:** código `201`, respuesta correcta e identificador generado.
    
- **Lectura:** código `200` y datos esperados.
    
- **Actualización parcial:** los campos omitidos conservan sus valores.
    
- **Eliminación:** código `204` y ausencia posterior del registro.
    
- **Validación:** entradas inválidas producen `422`.
    
- **Integridad:** registros duplicados o referencias inválidas producen el error HTTP definido por la aplicación.
    

## Reglas prácticas

- Usa una base de datos de prueba independiente; nunca apuntes las pruebas a la base de datos real.
    
- Sobrescribe la misma función de dependencia que utiliza el endpoint, no una función equivalente creada en el test.
    
- Evita ejecutar pruebas paralelas que modifiquen las sobrescrituras globales de la misma instancia de `app`.
    
- Si utilizas una dependencia de sesión con otro nombre o ubicación, importa exactamente esa función.
    
- Para probar servicios directamente, crea una `Session` de prueba sin pasar por `TestClient`.
    
- Añade pruebas para los casos límite, no solo para los escenarios exitosos.
    

## Prioriza

1. Configura `TestClient` y SQLite en memoria.
    
2. Sobrescribe `get_session`.
    
3. Prueba creación, lectura, actualización y eliminación.
    
4. Añade validaciones, errores de integridad y aislamiento entre pruebas.