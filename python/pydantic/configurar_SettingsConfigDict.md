# Pydantic Settings: configurar `SettingsConfigDict`

Utiliza `SettingsConfigDict` para controlar cómo `BaseSettings` carga, identifica y valida la configuración externa.

## Configuración práctica

```python
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    database_url: str
    debug: bool = False

    model_config = SettingsConfigDict(
        env_file=".env",
        env_file_encoding="utf-8",
        env_prefix="APP_",
        case_sensitive=False,
        extra="ignore",
    )
```

Archivo `.env`:

```
APP_DATABASE_URL=sqlite:///./app.db
APP_DEBUG=true
```

## Opciones principales

| Opción                  | Función                                                                                  |
| ----------------------- | ---------------------------------------------------------------------------------------- |
| `env_file`              | Define el archivo de variables de entorno.                                               |
| `env_file_encoding`     | Especifica la codificación del archivo.                                                  |
| `env_prefix`            | Añade un prefijo a los nombres de las variables.                                         |
| `case_sensitive`        | Controla la distinción entre mayúsculas y minúsculas.                                    |
| `extra="ignore"`        | Ignora variables adicionales del archivo `.env` que no correspondan a campos del modelo. |
| `extra="forbid"`        | Rechaza campos adicionales en las fuentes compatibles con esta validación.               |
| `secrets_dir`           | Define el directorio de archivos de secretos.                                            |
| `validate_default=True` | Valida los valores predeterminados.                                                      |

## Buenas prácticas

- Usa `env_prefix="APP_"` para evitar colisiones con variables de otros programas.
    
- Mantén `case_sensitive=False` si no necesitas distinguir mayúsculas y minúsculas.
    
- Elige `extra="forbid"` cuando quieras detectar errores tipográficos en `.env`; usa `ignore` cuando el archivo comparta variables con otras aplicaciones.
    
- Recuerda que `case_sensitive` puede comportarse de forma diferente según el sistema operativo.
    
- Define las opciones una sola vez en el modelo central de configuración.
    

**Regla:** configura `SettingsConfigDict` según el entorno y la política de validación de tu aplicación.

**Referencia:** `pydantic-settings` con Pydantic 2.x.