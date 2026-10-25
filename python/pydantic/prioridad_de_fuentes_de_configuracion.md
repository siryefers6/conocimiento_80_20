# Pydantic Settings: prioridad de fuentes de configuración

Comprende qué fuente prevalece cuando varias proporcionan un valor para el mismo campo. Por defecto, Pydantic Settings prioriza las fuentes en este orden, de mayor a menor:

1. Argumentos de inicialización.
    
2. Variables de entorno.
    
3. Archivo `.env`.
    
4. Directorio de secretos (`secrets_dir`).
    
5. Valores predeterminados del modelo.
    

## Ejemplo práctico

```python
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    debug: bool = False

    model_config = SettingsConfigDict(env_file=".env")

settings = Settings(debug=False)
```

Si el sistema define `DEBUG=true`, `.env` contiene `DEBUG=true` y el modelo establece `debug=False`, prevalece el argumento `debug=False`.

## Configura un directorio de secretos

```python
class Settings(BaseSettings):
    secret_key: str

    model_config = SettingsConfigDict(
        secrets_dir="/run/secrets",
    )
```

Pydantic busca un archivo llamado `secret_key` dentro de `/run/secrets` y utiliza su contenido como valor del campo, siempre que ninguna fuente de mayor prioridad lo sobrescriba.

## Reglas importantes

- Los valores predeterminados se utilizan cuando ninguna fuente de mayor prioridad proporciona el campo.
    
- Las fuentes se pueden reordenar mediante `settings_customise_sources()`.
    
- Los argumentos de inicialización no equivalen a variables de entorno: ambos son fuentes distintas.
    
- Protege los archivos de secretos y evita almacenarlos en Git.
    
- No confundas `secrets_dir` con un gestor de secretos remoto; la fuente predeterminada lee archivos locales.
    

**Regla:** define explícitamente qué fuente controla cada entorno y aprovecha la precedencia predeterminada antes de personalizarla.

**Referencia:** `pydantic-settings` con Pydantic 2.x.