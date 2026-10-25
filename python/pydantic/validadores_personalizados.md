# Pydantic: validadores personalizados

Utiliza `@field_validator` para aplicar reglas a campos individuales y `@model_validator` para validar relaciones entre varios campos.

## `@field_validator`

Valida o transforma el valor de un campo.

```python
from pydantic import BaseModel, field_validator

class Usuario(BaseModel):
    nombre: str

    @field_validator("nombre")
    @classmethod
    def validar_nombre(cls, valor: str) -> str:
        valor = valor.strip()
        if not valor:
            raise ValueError("El nombre no puede estar vacío")
        return valor
```

- Recibe el valor del campo.
    
- Devuelve el valor validado.
    
- Lanza `ValueError` cuando incumple una regla.
    
- Usa `mode="before"` para validar o transformar la entrada antes de la validación del tipo; el modo predeterminado es `"after"`.
    

## `@model_validator`

Comprueba reglas que relacionan varios campos.

```python
from pydantic import BaseModel, model_validator

class Reserva(BaseModel):
    inicio: int
    fin: int

    @model_validator(mode="after")
    def validar_intervalo(self):
        if self.fin <= self.inicio:
            raise ValueError("El fin debe ser posterior al inicio")
        return self
```

- `mode="after"`: recibe la instancia ya validada; devuelve `self`.
    
- `mode="before"`: recibe los datos de entrada antes de construir el modelo; normalmente devuelve los datos que se validarán.
    

## Reglas prácticas

- Usa `@field_validator` para formatos, normalización y restricciones específicas.
    
- Usa `@model_validator` para coherencia entre campos.
    
- Prefiere `ValueError` para errores de validación esperados.
    
- Evita duplicar restricciones que puedes expresar con `Field`.
    

**Referencia:** Pydantic 2.14.1.