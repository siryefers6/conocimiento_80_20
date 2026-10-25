# `asyncio.run()` — Ejecutar la corutina principal

Usa `asyncio.run()` para ejecutar una corutina desde un punto de entrada síncrono. Gestiona el bucle de eventos y devuelve el resultado final de la corutina.

```python
import asyncio

async def main():
    await asyncio.sleep(1)
    return "Completado"

resultado = asyncio.run(main())
print(resultado)  # Completado
```

## Reglas esenciales

- Pasa un **objeto corutina**, normalmente creado al llamar a una función `async def`.
    
- Ejecuta la corutina hasta que termine y devuelve su resultado.
    
- Gestiona el ciclo de vida del bucle de eventos.
    
- Úsalo como punto de entrada de una aplicación asíncrona.
    
- No lo llames desde un hilo que ya esté ejecutando un bucle de eventos; en ese contexto, utiliza `await`.
    

**Regla 80/20:** utiliza `asyncio.run(main())` para iniciar una aplicación asíncrona desde código síncrono. Evita crear un nuevo bucle de eventos para cada operación.