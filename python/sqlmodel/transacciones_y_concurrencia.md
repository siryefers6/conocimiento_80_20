# SQLModel: transacciones y concurrencia

**Versión:** SQLModel 0.0.48 · SQLAlchemy 2.x · **Referencia:** 24-10-2026

## 1. Transacciones: confirma o revierte cambios

Una transacción agrupa operaciones que deben completarse de forma coherente. Usa `commit()` para confirmar y `rollback()` para revertir los cambios pendientes.

```python
from sqlmodel import Session

with Session(engine) as session:
    try:
        session.add(producto)
        session.add(inventario)
        session.commit()
    except Exception:
        session.rollback()
        raise
```

Si falla una operación antes de confirmar, revierte la transacción. El bloque `with` cierra la sesión, pero no sustituye a `commit()`.

## 2. Define límites transaccionales

Usa `session.begin()` para establecer explícitamente una unidad de trabajo.

```python
with Session(engine) as session:
    with session.begin():
        session.add(producto)
        session.add(inventario)
```

Al finalizar correctamente, la transacción se confirma; si ocurre una excepción, se revierte.

- Agrupa operaciones que deben confirmarse juntas.
    
- Evita mantener transacciones abiertas durante llamadas HTTP, procesamiento lento o esperas externas.
    
- Define claramente qué capa controla la transacción para evitar `commit()` dispersos entre routers, servicios y repositorios.
    

## 3. Concurrencia: evita actualizaciones perdidas

Dos solicitudes pueden leer el mismo valor y sobrescribirse mutuamente.

Ejemplo: dos operaciones leen `stock = 10`; ambas descuentan 1 y guardan `9`. El resultado correcto debería ser `8`.

Para una modificación atómica, realiza la operación en SQL:

```python
from sqlalchemy import update

with Session(engine) as session:
    sentencia = (
        update(Producto)
        .where(
            Producto.id == producto_id,
            Producto.stock >= cantidad,
        )
        .values(stock=Producto.stock - cantidad)
    )

    resultado = session.exec(sentencia)
    session.commit()

    if resultado.rowcount == 0:
        raise ValueError("Producto inexistente o stock insuficiente")
```

La condición y el descuento se ejecutan como una sola sentencia. Ajusta la gestión del resultado si tu motor o configuración no proporciona un recuento fiable de filas afectadas.

## 4. Bloqueos y aislamiento

- **Bloqueo de escritura:** impide que ciertas operaciones concurrentes modifiquen simultáneamente los mismos datos.
    
- `**SELECT ... FOR UPDATE**`**:** permite solicitar bloqueos de filas en motores que lo soportan; su comportamiento depende del motor.
    
- **Nivel de aislamiento:** determina qué cambios concurrentes puede observar una transacción.
    
- **Optimistic locking:** detecta modificaciones concurrentes mediante una columna de versión y evita sobrescribir cambios silenciosamente.
    

SQLite utiliza bloqueos diferentes de PostgreSQL y no ofrece el mismo comportamiento de `SELECT ... FOR UPDATE`. Prueba la estrategia con el motor real de producción.

## Reglas prácticas

- Usa transacciones para operaciones que deben confirmarse juntas.
    
- Prefiere actualizaciones atómicas para contadores, inventario y otros valores numéricos compartidos.
    
- Usa restricciones de base de datos para garantizar invariantes, como `UNIQUE` y `CHECK`.
    
- Implementa control optimista con una versión cuando necesites detectar conflictos de actualización.
    
- Maneja bloqueos y errores de serialización con una política limitada de reintentos cuando sea seguro.
    
- No asumas que una transacción evita todos los conflictos: depende del nivel de aislamiento y de las operaciones ejecutadas.
    

## Prioriza

1. Domina `commit()`, `rollback()` y `session.begin()`.
    
2. Identifica operaciones susceptibles a actualizaciones perdidas.
    
3. Usa SQL atómico y restricciones para proteger los datos.
    
4. Aprende bloqueos y control optimista según las necesidades del motor.