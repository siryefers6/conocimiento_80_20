# `contextvars` — Mantener contexto local por tarea asíncrona

Usa `contextvars.ContextVar` para almacenar datos asociados al contexto de ejecución, como identificadores de solicitud, trazas o información de diagnóstico, sin pasar esos valores como argumentos por todas las funciones.

## Ejemplo mínimo

```python
import asyncio
from contextvars import ContextVar

solicitud_id = ContextVar("solicitud_id", default="sin-id")

async def procesar():
    print(solicitud_id.get())

async def main():
    token = solicitud_id.set("req-123")
    try:
        await asyncio.gather(procesar(), procesar())
    finally:
        solicitud_id.reset(token)

asyncio.run(main())
```

Ambas tareas heredan el contexto de `main()` y obtienen `req-123`.

## Operaciones esenciales

- `**ContextVar(nombre)**`**:** declara una variable de contexto.
    
- `**variable.set(valor)**`**:** asigna un valor y devuelve un token para restaurarlo.
    
- `**variable.get()**`**:** obtiene el valor del contexto actual.
    
- `**variable.reset(token)**`**:** restaura el valor anterior.
    
- `**asyncio.create_task()**`**:** crea tareas que normalmente heredan una copia del contexto actual.
    

## Regla 80/20

Utiliza `contextvars` para propagar identificadores de solicitudes y datos de trazabilidad entre corutinas. Restaura los valores con `reset(token)` cuando debas limitar su alcance y evitar contaminación entre operaciones.