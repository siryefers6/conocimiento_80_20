# Pydantic Settings: validar la configuración al iniciar

Define campos tipados, valores predeterminados y restricciones para detectar errores de configuración antes de ejecutar la aplicación.

## Define restricciones

```python
from pydantic import Field, SecretStr
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    database_url: str
    secret_key: SecretStr
    debug: bool = False
    port: int = Field(default=8000, ge=1, le=65535)

    model_config = SettingsConfigDict(
        env_file=".env",
        extra="ignore",
    )

settings = Settings()
```

- `database_url`: obligatorio; falla si falta.
    
- `secret_key`: obligatorio y protegido frente a la exposición accidental al representarlo.
    
- `debug`: usa `False` si no se proporciona.
    
- `port`: acepta enteros entre `1` y `65535`.
    

## Controla el inicio de la aplicación

```python
from pydantic import ValidationError

try:
    settings = Settings()
except ValidationError:
    raise SystemExit("Error: configuración inválida o incompleta")
```

Si falta una variable obligatoria o un valor incumple las restricciones, Pydantic genera `ValidationError`. La aplicación debe detener el inicio.

## Buenas prácticas

- Define sin valor predeterminado las variables obligatorias.
    
- Usa `Field` para restringir rangos y longitudes.
    
- Utiliza `SecretStr` para claves y contraseñas.
    
- No muestres valores sensibles ni el contenido completo de la excepción en registros públicos.
    
- Crea y valida la configuración durante el inicio, antes de aceptar solicitudes.
    

**Regla:** falla rápido ante una configuración inválida; no sustituyas silenciosamente secretos o parámetros críticos por valores ficticios.

**Referencia:** Pydantic 2.14.1 y `pydantic-settings`.