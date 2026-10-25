# Pydantic Settings: mapear variables de entorno

Define campos tipados en `BaseSettings` y carga sus valores desde variables de entorno. Pydantic Settings convierte los valores compatibles y valida los tipos.

## Define la configuración

```python
# app/config.py
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    database_url: str
    debug: bool = False
    secret_key: str

    model_config = SettingsConfigDict(
        env_file=".env",
        env_file_encoding="utf-8",
    )

settings = Settings()
```

## Configura las variables

Archivo `.env`:

```
DATABASE_URL=sqlite:///./app.db
DEBUG=false
SECRET_KEY=cambia-esta-clave
```

Los nombres de las variables se corresponden con los campos sin distinguir mayúsculas y minúsculas por defecto.

## Accede a la configuración

```python
print(settings.database_url)
print(settings.debug)
```

- `DATABASE_URL`: dirección de conexión a la base de datos.
    
- `DEBUG`: activa o desactiva el modo de depuración.
    
- `SECRET_KEY`: clave secreta de la aplicación.
    

## Reglas importantes

- Usa `str`, `bool`, `int` y otros tipos para validar la configuración.
    
- Define sin valor predeterminado los campos que sean obligatorios.
    
- No subas `.env` al repositorio; inclúyelo en `.gitignore`.
    
- Proporciona las claves secretas mediante el entorno de despliegue o un gestor de secretos.
    
- Recuerda que las variables de entorno reales tienen prioridad sobre los valores de `.env`.
    

**Regla:** centraliza la configuración en `BaseSettings` y evita acceder directamente a `os.environ` desde cada módulo.

**Referencia:** `pydantic-settings` con Pydantic 2.x.