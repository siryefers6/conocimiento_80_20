# Pytest: probar validaciones de Pydantic y Settings

Automatiza pruebas para entradas inválidas, valores límite y configuraciones incompletas. Comprueba tanto los modelos de datos como la configuración de la aplicación.

## 1. Prueba entradas inválidas y límites

```python
import pytest
from pydantic import BaseModel, Field, ValidationError

class Producto(BaseModel):
    cantidad: int = Field(ge=1, le=100)

def test_cantidad_limite():
    assert Producto(cantidad=1).cantidad == 1
    assert Producto(cantidad=100).cantidad == 100

@pytest.mark.parametrize("cantidad", [0, 101, "invalido"])
def test_cantidad_invalida(cantidad):
    with pytest.raises(ValidationError):
        Producto(cantidad=cantidad)
```

## 2. Prueba configuraciones incompletas

```python
from pydantic import SecretStr
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    secret_key: SecretStr

def test_faltan_variables(monkeypatch):
    monkeypatch.delenv("DATABASE_URL", raising=False)
    monkeypatch.delenv("SECRET_KEY", raising=False)

    with pytest.raises(ValidationError):
        Settings(_env_file=None)
```

Importa `ValidationError` desde `pydantic` en este segundo ejemplo también.

`_env_file=None` evita que un archivo `.env` local complete los valores ausentes durante la prueba.

## 3. Comprueba la precedencia de configuración

Verifica que las variables de entorno prevalezcan sobre `.env` y que los argumentos de inicialización prevalezcan sobre las variables de entorno.

## Buenas prácticas

- Usa `pytest.mark.parametrize` para cubrir varios casos.
    
- Prueba los límites exactos y los valores inmediatamente fuera del rango.
    
- Usa `monkeypatch` para aislar las variables de entorno.
    
- Evita depender de credenciales o archivos de configuración reales.
    
- Comprueba también los valores predeterminados y los campos opcionales.
    

**Regla:** prueba casos válidos, inválidos, límites y ausencia de configuración para detectar fallos antes del despliegue.