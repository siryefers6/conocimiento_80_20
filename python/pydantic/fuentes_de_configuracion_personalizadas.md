# Pydantic Settings: fuentes de configuración personalizadas

Utiliza `settings_customise_sources()` para incorporar fuentes propias o modificar la prioridad de las fuentes estándar. Aplícalo solo cuando variables de entorno, archivos `.env` y archivos de secretos no sean suficientes.

## Ejemplo: fuente personalizada

```python
from typing import Any

from pydantic_settings import (
    BaseSettings,
    PydanticBaseSettingsSource,
    SettingsConfigDict,
)

class Settings(BaseSettings):
    app_name: str = "Aplicación"

    model_config = SettingsConfigDict(env_prefix="APP_")

    @classmethod
    def settings_customise_sources(
        cls,
        settings_cls: type[BaseSettings],
        init_settings: PydanticBaseSettingsSource,
        env_settings: PydanticBaseSettingsSource,
        dotenv_settings: PydanticBaseSettingsSource,
        file_secret_settings: PydanticBaseSettingsSource,
    ) -> tuple[PydanticBaseSettingsSource, ...]:
        return (
            init_settings,
            env_settings,
            dotenv_settings,
            file_secret_settings,
        )
```

Este ejemplo conserva las fuentes y su prioridad predeterminada; úsalo como punto de partida para personalizarlas.

## Conceptos clave

- `settings_customise_sources()`: define qué fuentes se consultan y en qué orden.
    
- Cada fuente devuelve valores que Pydantic combina y valida.
    
- Las fuentes anteriores en la tupla tienen mayor prioridad.
    
- Puedes añadir una fuente personalizada heredando de `PydanticBaseSettingsSource` o utilizando una implementación compatible.
    

**Regla:** domina primero `BaseSettings`, `.env`, variables de entorno y `secrets_dir`. Personaliza las fuentes únicamente cuando exista un requisito concreto.

**Referencia:** Pydantic 2.14.1 y `pydantic-settings`.