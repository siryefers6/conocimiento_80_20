# Pydantic Settings: configuración anidada con `env_nested_delimiter`

Agrupa parámetros relacionados en modelos anidados y utiliza `env_nested_delimiter` para convertir variables de entorno en estructuras jerárquicas.

## Define modelos anidados

```python
from pydantic import BaseModel
from pydantic_settings import BaseSettings, SettingsConfigDict

class DatabaseConfig(BaseModel):
    host: str = "localhost"
    port: int = 5432
    name: str = "app"

class Settings(BaseSettings):
    database: DatabaseConfig

    model_config = SettingsConfigDict(
        env_nested_delimiter="__",
    )

settings = Settings()
```

## Configura variables de entorno

```
DATABASE__HOST=192.168.1.10
DATABASE__PORT=5433
DATABASE__NAME=produccion
```

Pydantic interpreta `__` como separador y construye el objeto `database` con sus respectivos campos.

## Accede a los valores

```python
print(settings.database.host)
print(settings.database.port)
print(settings.database.name)
```

## Reglas importantes

- Usa `BaseModel` para agrupar parámetros relacionados.
    
- Configura `env_nested_delimiter="__"` en `SettingsConfigDict`.
    
- Mantén los nombres de variables consistentes con los campos del modelo.
    
- Pydantic convierte los valores de entorno a los tipos declarados y valida el resultado.
    
- Las variables anidadas específicas pueden sobrescribir valores procedentes de una variable JSON correspondiente al modelo completo.
    

**Regla:** agrupa la configuración por responsabilidad, por ejemplo `database`, `security` y `email`, para mantener una estructura clara y extensible.

**Referencia:** Pydantic 2.14.1 y `pydantic-settings`.