# SQLModel: validación de datos con Pydantic

**Versión:** SQLModel 0.0.48 · **Referencia:** 24-10-2026

## Concepto

Valida tipos, campos obligatorios, formatos y restricciones antes de persistir datos. SQLModel utiliza Pydantic para validar los modelos, pero **la validación de entrada no sustituye las restricciones de la base de datos**.

## 1. Validación mediante tipos y `Field()`

```python
from sqlmodel import SQLModel, Field


class ProductoCreate(SQLModel):
    nombre: str = Field(min_length=2, max_length=100)
    precio: float = Field(gt=0)
    stock: int = Field(ge=0)
```

- `min_length` / `max_length`: limita la longitud del texto.
    
- `gt` / `ge`: exige un valor mayor que cero o mayor o igual que cero.
    
- Los campos sin valor predeterminado son obligatorios.
    
- Los tipos permiten validar y convertir datos compatibles.
    

## 2. Validar formatos con `field_validator`

```python
from pydantic import field_validator
from sqlmodel import SQLModel, Field


class UsuarioCreate(SQLModel):
    correo: str = Field(min_length=5)

    @field_validator("correo")
    @classmethod
    def validar_correo(cls, valor: str) -> str:
        valor = valor.strip().lower()

        if "@" not in valor:
            raise ValueError("Correo inválido")

        return valor
```

Usa `field_validator` para reglas personalizadas. Para validar correctamente un correo electrónico, prefiere un tipo especializado como `EmailStr` de Pydantic, con su dependencia correspondiente instalada.

## 3. Separar entrada y persistencia

```python
from sqlmodel import SQLModel, Field


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str
    precio: float


class ProductoCreate(SQLModel):
    nombre: str = Field(min_length=2)
    precio: float = Field(gt=0)


datos = ProductoCreate(nombre="Teclado", precio=25)
producto = Producto.model_validate(datos)
```

Usa modelos independientes para creación, lectura y actualización. El modelo de tabla representa la persistencia; el modelo de entrada define qué datos acepta la aplicación.

## Reglas prácticas

- Valida los datos antes de llamar a `session.add()` y `session.commit()`.
    
- Usa `Field()` para restricciones declarativas y `field_validator` para reglas personalizadas.
    
- Usa `model_validate()` para validar datos al construir modelos a partir de otros objetos o estructuras compatibles.
    
- No confundas validación con restricciones SQL: utiliza `UNIQUE`, claves foráneas y restricciones de base de datos cuando corresponda.
    
- Para actualizaciones parciales, declara campos opcionales en el modelo de entrada y utiliza `model_dump(exclude_unset=True)` para obtener únicamente los campos enviados.