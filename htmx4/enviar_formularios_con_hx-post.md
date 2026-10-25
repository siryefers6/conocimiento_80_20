# HTMX 4: enviar formularios con `hx-post`

Envía los campos de un formulario a FastAPI mediante HTTP `POST`. Recibe HTML y actualiza la interfaz sin recargar la página.

## Ejemplo mínimo

**FastAPI —** `**main.py**`

```python
from fastapi import FastAPI, Form
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.post("/pedidos", response_class=HTMLResponse)
def crear_pedido(numero: str = Form()):
    return f"<p>Pedido #{numero} registrado.</p>"
```

**HTML**

```html
<form hx-post="/pedidos" hx-target="#resultado">
    <input name="numero" required>
    <button type="submit">Registrar</button>
</form>

<div id="resultado"></div>
```

Instala las dependencias:

```
uv add fastapi uvicorn python-multipart
```

## Flujo de ejecución

1. El usuario envía el formulario.
    
2. HTMX transmite los campos con `name`.
    
3. FastAPI recibe y valida los datos.
    
4. El servidor devuelve HTML.
    
5. HTMX inserta la respuesta en `#resultado`.
    

## Reglas operativas

- Usa `hx-post` en el formulario.
    
- Asigna `name` a cada campo que debas enviar.
    
- Define `hx-target` para controlar dónde aparece la respuesta.
    
- Valida siempre en FastAPI; `required` solo aporta validación del navegador.
    
- Protege las operaciones contra CSRF cuando corresponda.
    

**Principio:** el formulario recopila; HTMX transporta; FastAPI valida y procesa.