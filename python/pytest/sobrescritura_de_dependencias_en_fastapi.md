# Sobrescritura de dependencias en FastAPI

Utiliza `app.dependency_overrides` para sustituir dependencias reales por versiones controladas durante las pruebas, como sesiones de base de datos, autenticación o servicios externos.

## 1. Sustituir una dependencia

```python
from fastapi import Depends, FastAPI
from fastapi.testclient import TestClient

app = FastAPI()

def obtener_usuario():
    return {"id": 1, "nombre": "Real"}

@app.get("/usuario")
def usuario(actual=Depends(obtener_usuario)):
    return actual

def test_usuario():
    app.dependency_overrides[obtener_usuario] = (
        lambda: {"id": 99, "nombre": "Prueba"}
    )

    try:
        response = TestClient(app).get("/usuario")
        assert response.status_code == 200
        assert response.json()["id"] == 99
    finally:
        app.dependency_overrides.clear()
```

FastAPI utiliza la función original como clave y la sustituta como valor.

## 2. Casos habituales

- **Base de datos:** proporcionar una sesión conectada a una base exclusiva de pruebas.
    
- **Autenticación:** simular usuarios autenticados o permisos específicos.
    
- **Servicios externos:** evitar llamadas de red y devolver respuestas controladas.
    
- **Configuración:** sustituir dependencias que proporcionan valores de entorno.
    

## 3. Reglas

- Sustituye la función que aparece en `Depends()`, no su resultado.
    
- Limpia las sobrescrituras con `app.dependency_overrides.clear()` o restaura únicamente las modificadas.
    
- Usa `try/finally` o una fixture de Pytest para garantizar la limpieza.
    
- Evita compartir sobrescrituras mutables entre pruebas sin control.
    
- No confundas la sobrescritura de dependencias con `unittest.mock.patch`: FastAPI permite sustituir directamente sus dependencias declaradas.
    

**Prioriza:** utilizar una fixture para configurar y restaurar `app.dependency_overrides` en pruebas de API que necesiten sesiones de prueba, usuarios simulados o servicios aislados.