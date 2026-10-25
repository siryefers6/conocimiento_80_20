# Pydantic Settings: gestionar secretos y credenciales

Carga contraseñas, claves API y credenciales desde variables de entorno o archivos de secretos. Evita incluir valores sensibles en el código fuente o en Git.

## 1. Protege los campos sensibles

Utiliza `SecretStr` para impedir que los secretos se muestren accidentalmente al imprimir el modelo.

```python
from pydantic import SecretStr
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_password: SecretStr
    api_key: SecretStr

settings = Settings()

print(settings.api_key)          # **********
print(settings.api_key.get_secret_value())  # Valor real
```

Usa `get_secret_value()` únicamente cuando necesites entregar el secreto al componente correspondiente.

## 2. Carga secretos desde variables de entorno

```
DATABASE_PASSWORD=contraseña-local
API_KEY=clave-local
```

Define las variables en el entorno de ejecución y evita incluir el archivo `.env` real en el repositorio.

## 3. Utiliza archivos de secretos

Configura un directorio con un archivo por secreto:

```python
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    database_password: SecretStr
    api_key: SecretStr

    model_config = SettingsConfigDict(
        secrets_dir="/run/secrets",
    )
```

Estructura esperada:

```
/run/secrets/
├── database_password
└── api_key
```

Pydantic carga el contenido de cada archivo si ninguna fuente de mayor prioridad proporciona ese campo.

## 4. Evita filtraciones

- Añade `.env` a `.gitignore`.
    
- Mantén `.env.example` con nombres de variables y valores ficticios.
    
- Limita los permisos de los archivos de secretos.
    
- No registres secretos ni los devuelvas en respuestas HTTP.
    
- Si una credencial llega a Git, revócala y genera una nueva; eliminar el archivo del último commit no elimina necesariamente el historial.
    

**Regla:** separa los secretos del código, limita su exposición y utiliza un gestor de secretos en producción cuando esté disponible.

**Referencia:** Pydantic 2.14.1 y `pydantic-settings`.