# Pydantic Settings: configuración centralizada con `BaseSettings`

Instala `pydantic-settings` por separado y utiliza `BaseSettings` para cargar y validar la configuración de la aplicación desde variables de entorno y otras fuentes.

## Instalación

```sh
uv add pydantic-settings
```

## Define la configuración

```python
# app/config.py
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    app_name: str = "Mi aplicación"
    debug: bool = False
    database_url: str = "sqlite:///./app.db"
    uploads_dir: str = "uploads"

    model_config = SettingsConfigDict(
        env_prefix="APP_",
        extra="ignore",
    )

settings = Settings()
```

## Carga variables de entorno

```sh
export APP_DEBUG=true
export APP_DATABASE_URL="sqlite:///./produccion.db"
```

```python
from app.config import settings

print(settings.debug)
print(settings.database_url)
```

`BaseSettings` convierte los valores de las variables de entorno a los tipos declarados y valida la configuración al crear la instancia.

## Buenas prácticas

- Centraliza la configuración en un módulo, como `app/config.py`.
    
- Define valores predeterminados solo cuando sean apropiados.
    
- Usa `env_prefix` para evitar conflictos entre variables.
    
- No guardes contraseñas ni claves secretas directamente en el código.
    
- Crea una instancia reutilizable de `Settings` para evitar lecturas repetidas.
    
- Separa la configuración por entorno mediante variables de entorno o archivos `.env`.
    

**Regla:** utiliza `BaseSettings` para configuración externa y `BaseModel` para validar datos de negocio o solicitudes.

**Referencia:** Pydantic 2.14.1 · paquete independiente `pydantic-settings`.