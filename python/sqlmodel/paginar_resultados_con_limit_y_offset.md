# SQLModel: paginar resultados con `limit()` y `offset()`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `limit()` para establecer el máximo de registros por página y `offset()` para indicar cuántos registros omitir.

## 1. Consultar una página

```python
from sqlmodel import Session, select

pagina = 2
por_pagina = 10

statement = (
    select(Producto)
    .order_by(Producto.id)
    .offset((pagina - 1) * por_pagina)
    .limit(por_pagina)
)

with Session(engine) as session:
    productos = session.exec(statement).all()
```

- `limit(10)`: devuelve como máximo 10 registros.
    
- `offset(10)`: omite los primeros 10 registros.
    
- `pagina`: comienza en 1.
    
- `order_by()`: establece un orden consistente para paginar.
    

## 2. Calcular el desplazamiento

Utiliza esta fórmula:

[  
\text{offset}=(\text{pagina}-1)\times\text{por_pagina}  
]

| Página | Registros por página | `offset` |
| ------ | -------------------- | -------- |
| 1      | 10                   | 0        |
| 2      | 10                   | 10       |
| 3      | 10                   | 20       |

## 3. Validar los parámetros

```python
pagina = max(1, pagina)
por_pagina = min(max(1, por_pagina), 100)
```

Impide páginas menores que 1 y limita el tamaño máximo para evitar consultas excesivas.

## Reglas prácticas

- Aplica siempre `order_by()` para obtener páginas reproducibles.
    
- Limita el tamaño máximo de página.
    
- Considera que las inserciones y eliminaciones concurrentes pueden desplazar resultados entre páginas.
    
- Para tablas muy grandes, evalúa la paginación por cursor mediante una clave ordenable.
    

**Prioriza:** fórmula de `offset`, límites de seguridad y diferencias entre paginación por desplazamiento y por cursor.