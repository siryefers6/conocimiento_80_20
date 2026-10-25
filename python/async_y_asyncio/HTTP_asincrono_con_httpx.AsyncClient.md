# HTTP asíncrono con `httpx.AsyncClient`

Realiza solicitudes HTTP sin bloquear el event loop usando `httpx.AsyncClient` y `await`.

```python
import asyncio
import httpx

async def obtener_usuario(cliente: httpx.AsyncClient, id: int) -> dict:
    respuesta = await cliente.get(
        f"https://jsonplaceholder.typicode.com/users/{id}",
        timeout=10.0,
    )
    respuesta.raise_for_status()
    return respuesta.json()

async def main():
    async with httpx.AsyncClient() as cliente:
        usuarios = await asyncio.gather(
            obtener_usuario(cliente, 1),
            obtener_usuario(cliente, 2),
        )
        print(usuarios)

asyncio.run(main())
```

- **Reutiliza el cliente:** comparte una instancia para aprovechar las conexiones HTTP.
    
- **Cierra los recursos:** usa `async with` para cerrar el cliente correctamente.
    
- **Controla errores:** `raise_for_status()` detecta respuestas HTTP de error; captura `httpx.HTTPError` cuando necesites recuperarte.
    
- **Define límites:** configura `timeout` y, si corresponde, límites de conexiones.
    
- **Evita bloquear:** usa `await` en las operaciones HTTP; no llames a clientes síncronos dentro de una corrutina.
    

Instala la dependencia con `uv add httpx`.