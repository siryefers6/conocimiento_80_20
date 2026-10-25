# Pydantic Settings: instancia única de configuración

Crea una instancia compartida de `Settings` durante la importación del módulo para centralizar la configuración y evitar reconstruirla en cada operación.

## Define la instancia

```python
# app/config.py
from functools import lru_cache

from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    database_url: str
    debug: bool = False

    model_config = SettingsConfigDict(env_file=".env")

@lru_cache
def get_settings() -> Settings:
    return Settings()
```

## Reutiliza la configuración

```python
# app/main.py
from app.config import get_settings

settings = get_settings()

print(settings.database_url)
print(settings.debug)
```

`@lru_cache` conserva la instancia creada y evita volver a leer y validar las fuentes en llamadas posteriores dentro del mismo proceso.

## Buenas prácticas

- Centraliza `Settings` en un módulo.
    
- Usa `get_settings()` para compartir la configuración.
    
- Valida la configuración al iniciar la aplicación si los campos obligatorios deben estar disponibles desde el principio.
    
- Recuerda que los cambios en las variables de entorno no se reflejan automáticamente después de almacenar la instancia en caché.
    
- En pruebas, limpia la caché con `get_settings.cache_clear()` cuando necesites cargar una configuración diferente.
    

**Regla:** reutiliza una instancia por proceso y evita crear `Settings()` dentro de cada función o solicitud.

**Referencia:** Pydantic 2.14.1 y `pydantic-settings`.