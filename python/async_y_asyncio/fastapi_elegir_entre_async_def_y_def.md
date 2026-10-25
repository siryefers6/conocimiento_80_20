# FastAPI — Elegir entre `async def` y `def`

Usa `async def` cuando el endpoint realiza operaciones de entrada/salida con bibliotecas asíncronas. Usa `def` cuando dependas de bibliotecas síncronas que bloquean durante esas operaciones.

```python
from fastapi import FastAPI
import httpx

app = FastAPI()

@app.get("/usuarios/{id}")
async def obtener_usuario(id: int):
    async with httpx.AsyncClient() as cliente:
        respuesta = await cliente.get(
            f"https://api.example.com/usuarios/{id}"
        )
        respuesta.raise_for_status()
        return respuesta.json()

@app.get("/saludo")
def saludo():
    return {"mensaje": "Hola"}
```

- `**async def**`**:** úsalo con `await` y bibliotecas asíncronas, como `httpx.AsyncClient` o `AsyncSession`.
    
- `**def**`**:** úsalo cuando una biblioteca síncrona bloqueante no tenga alternativa asíncrona. FastAPI ejecuta estos endpoints en un thread pool.
    
- **No bloquees el event loop:** evita llamadas síncronas de red o bases de datos dentro de `async def`.
    
- **No añadas asincronía innecesaria:** una función que solo calcula o devuelve datos puede ser síncrona o asíncrona según el resto del flujo.
    
- **CPU intensiva:** para cálculos pesados, considera procesos separados; `async def` no paraleliza automáticamente el trabajo.
    

**Regla práctica:** elige según las bibliotecas utilizadas y dónde ocurre la espera, no por una preferencia general por `async def`.