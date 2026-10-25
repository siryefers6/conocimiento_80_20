# SQLModel: validadores de Pydantic para reglas de negocio

**Versión:** SQLModel 0.0.48 · Pydantic 2.x · **Referencia:** 24-10-2026

## 1. `field_validator`: validar un campo

Usa `field_validator` para normalizar o comprobar un valor individual.

```python
from pydantic import field_validator
from sqlmodel import SQLModel, Field


class ProductoCreate(SQLModel):
    nombre: str = Field(min_length=2, max_length=100)
    precio: float = Field(gt=0)

    @field_validator("nombre")
    @classmethod
    def normalizar_nombre(cls, valor: str) -> str:
        valor = valor.strip()

        if not valor:
            raise ValueError("El nombre es obligatorio")

        return valor
```

- Normaliza los datos antes de utilizarlos.
    
- Usa `ValueError` para rechazar valores inválidos.
    
- Combina validadores con `Field()` para aplicar restricciones declarativas.
    

## 2. `model_validator`: validar varios campos

Usa `model_validator(mode="after")` cuando una regla dependa de la relación entre varios campos.

```python
from pydantic import model_validator
from sqlmodel import SQLModel, Field


class PedidoCreate(SQLModel):
    subtotal: float = Field(ge=0)
    descuento: float = Field(default=0, ge=0)

    @model_validator(mode="after")
    def validar_descuento(self):
        if self.descuento > self.subtotal:
            raise ValueError("El descuento supera el subtotal")

        return self
```

El validador recibe la instancia ya construida y debe devolverla si la validación es correcta.

## 3. `mode="before"`: validar datos de entrada

Usa `model_validator(mode="before")` para examinar o transformar los datos antes de la validación de campos.

```python
from typing import Any
from pydantic import model_validator
from sqlmodel import SQLModel


class UsuarioCreate(SQLModel):
    nombre: str
    correo: str

    @model_validator(mode="before")
    @classmethod
    def comprobar_entrada(cls, datos: Any) -> Any:
        if isinstance(datos, dict) and "nombre" not in datos:
            raise ValueError("Falta el nombre")

        return datos
```

Prefiere `mode="after"` para reglas entre campos ya validados y `mode="before"` cuando necesites inspeccionar los datos originales.

## Reglas prácticas

- `**field_validator**`**:** valida o normaliza un campo específico.
    
- `**model_validator**`**:** valida condiciones que involucran varios campos.
    
- `**Field()**`**:** define límites simples, como `gt=0`, `min_length` o `max_length`.
    
- `**ValueError**`**:** comunica una validación fallida; Pydantic la incorpora a sus errores de validación.
    
- **Persistencia:** valida antes de guardar, pero repite las restricciones críticas en la base de datos cuando corresponda.
    
- **Arquitectura:** reserva los validadores para reglas locales a los datos. Para reglas que consultan la base de datos, como comprobar que un código sea único, utiliza la capa de servicio y una restricción `UNIQUE` en la base de datos.