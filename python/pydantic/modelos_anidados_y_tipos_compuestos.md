# Pydantic: modelos anidados y tipos compuestos

Combina modelos y tipos especializados para representar estructuras complejas y validar contratos de datos.

## Ejemplo práctico

```python
from datetime import date, datetime
from decimal import Decimal
from enum import Enum
from typing import Literal
from uuid import UUID

from pydantic import BaseModel

class Estado(str, Enum):
    pendiente = "pendiente"
    enviado = "enviado"

class Articulo(BaseModel):
    nombre: str
    precio: Decimal
    etiquetas: list[str]

class Pedido(BaseModel):
    id: UUID
    fecha: date
    creado_en: datetime
    estado: Estado
    moneda: Literal["CLP", "USD"]
    cliente: str | int
    articulos: list[Articulo]
    metadatos: dict[str, str]
```

## Tipos compuestos principales

- **Modelos anidados:** reutiliza modelos dentro de otros para validar estructuras jerárquicas.
    
- `**list[T]**`**:** valida cada elemento de una lista.
    
- `**dict[K, V]**`**:** valida las claves y los valores del diccionario.
    
- `**Enum**`**:** limita los valores a opciones predefinidas.
    
- `**Literal**`**:** acepta únicamente los valores declarados.
    
- `**A | B**`**:** permite varios tipos alternativos.
    
- `**datetime**` **y** `**date**`**:** representan fecha y fecha con hora.
    
- `**UUID**`**:** representa identificadores únicos.
    
- `**Decimal**`**:** representa números decimales con precisión controlada.
    

## Ejemplo de validación

```python
pedido = Pedido(
    id="550e8400-e29b-41d4-a716-446655440000",
    fecha="2026-10-24",
    creado_en="2026-10-24T10:30:00",
    estado="pendiente",
    moneda="CLP",
    cliente=123,
    articulos=[
        {
            "nombre": "Teclado",
            "precio": "19990.00",
            "etiquetas": ["periférico"],
        }
    ],
    metadatos={"origen": "web"},
)
```

Pydantic convierte entradas compatibles y valida los tipos de cada nivel. Si un campo incumple su contrato, genera `ValidationError`.

**Regla:** utiliza modelos anidados para estructuras reutilizables, `Enum` para conjuntos de opciones y `Literal` para valores constantes. Prefiere `Decimal` para importes monetarios.

**Referencia:** Pydantic 2.14.1.