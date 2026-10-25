# Pydantic Settings: cargar configuración desde `.env`

Configura `env_file` mediante `SettingsConfigDict` para cargar variables desde un archivo local. Las variables de entorno del sistema tienen prioridad sobre los valores del archivo `.env`.

## Configuración

```python
# app/config.py
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    database_url: str
    debug: bool = False

    model_config = SettingsConfigDict(
        env_file=".env",
        env_file_encoding="utf-8",
    )

settings = Settings()
```

Archivo `.env`, ubicado en el directorio de trabajo:

```
DATABASE_URL=sqlite:///./app.db
DEBUG=false
```

## Prioridad de valores

De mayor a menor prioridad:

1. Argumentos pasados al crear `Settings`.
    
2. Variables de entorno del sistema.
    
3. Archivo `.env`.
    
4. Valores predeterminados del modelo.
    

```sh
export DEBUG=true
uv run python -c "from app.config import settings; print(settings.debug)"
# True
```

Aunque `.env` contenga `DEBUG=false`, prevalece la variable de entorno `DEBUG=true`.

## Resolución de rutas

- `env_file=".env"`: resuelve la ruta relativa respecto del directorio de trabajo actual.
    
- No busca automáticamente el archivo `.env` en directorios superiores.
    
- Usa una ruta absoluta si necesitas eliminar la dependencia del directorio de ejecución.
    

```python
from pathlib import Path
from pydantic_settings import SettingsConfigDict

BASE_DIR = Path(__file__).resolve().parent.parent

model_config = SettingsConfigDict(
    env_file=BASE_DIR / ".env",
    env_file_encoding="utf-8",
)
```

## Buenas prácticas

- Añade `.env` a `.gitignore`.
    
- Mantén `.env.example` sin secretos reales.
    
- Define `env_file=None` si quieres desactivar la carga del archivo para una instancia concreta.
    
- Recuerda que un archivo `.env` inexistente no genera un error por sí solo; sí fallará la validación si faltan campos obligatorios.
    

**Regla:** utiliza `.env` para desarrollo local y variables de entorno del sistema para configurar despliegues.

**Referencia:** `pydantic-settings` con Pydantic 2.x.