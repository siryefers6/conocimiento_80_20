Envía datos al servidor mediante una petición HTTP `POST`. Úsalo para registrar formularios sin recargar toda la página.

## Ejemplo mínimo con FastAPI

Backend — `main.py`

```python
from fastapi import FastAPI, Form
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.post("/pedidos", response_class=HTMLResponse)
def crear_pedido(numero: str = Form()):
    return f"<p>Pedido {numero} registrado.</p>"
```

Instala las dependencias:

```
uv add fastapi uvicorn python-multipart
```

HTML

```html
<form hx-post="/pedidos" hx-target="#resultado">
    <input name="numero" required>
    <button type="submit">Registrar</button>
</form>

<div id="resultado"></div>
```

## Flujo de ejecución

1. El usuario envía el formulario.
    
2. HTMX envía los campos mediante `POST`.
    
3. FastAPI recibe y procesa los datos.
    
4. El servidor devuelve HTML.
    
5. HTMX inserta la respuesta en `#resultado`.
    

## Reglas operativas

- Usa `hx-post` para crear registros o ejecutar operaciones que modifican datos.
    
- Define `name` en cada campo que quieras enviar.
    
- Usa `hx-target` para elegir dónde mostrar la respuesta.
    
- Valida los datos en FastAPI; `required` en HTML no sustituye la validación del servidor.
    
- Implementa protección CSRF cuando corresponda a tu mecanismo de autenticación.
    

Principio: el formulario recopila; HTMX transporta; FastAPI valida y procesa.