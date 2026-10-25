# Pydantic Settings: configuración por entorno

Separa los valores de desarrollo, pruebas y producción mediante variables de entorno. Mantén un único modelo `Settings` y modifica sus valores según el entorno de ejecución.

## Define la configuración

```python
# app/config.py
from typing import Literal

from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    environment: Literal["development", "test", "production"] = "development"
    debug: bool = False
    database_url: str

    model_config = SettingsConfigDict(
        env_prefix="APP_",
        env_file=".env",
        extra="ignore",
    )

settings = Settings()
```

## Configura cada entorno

Desarrollo (`.env` local):

```
APP_ENVIRONMENT=development
APP_DEBUG=true
APP_DATABASE_URL=sqlite:///./dev.db
```

Pruebas:

```
APP_ENVIRONMENT=test APP_DEBUG=false APP_DATABASE_URL=sqlite:///./test.db uv run pytest
```

Producción: define las variables en el entorno de despliegue y utiliza una base de datos y secretos propios de producción.

## Reglas prácticas

- Usa una base de datos independiente para pruebas.
    
- No guardes credenciales reales en Git.
    
- Desactiva `debug` en producción.
    
- No dependas de `.env` para configurar producción.
    
- Valida los valores al iniciar y falla si falta una variable obligatoria.
    
- No uses `environment` como sustituto de controles de seguridad.
    

**Regla:** separa los valores por entorno sin duplicar innecesariamente la lógica de configuración.

**Referencia:** Pydantic 2.14.1 y `pydantic-settings`.