# HTMX 4: mostrar errores de validación en formularios

Devuelve los errores como HTML y colócalos dentro del formulario usando `hx-target`. Conserva los campos para que el usuario corrija los datos sin recargar la página.

## Ejemplo con FastAPI y Pydantic

`**main.py**`

```python
from fastapi import FastAPI, Form
from fastapi.responses import HTMLResponse
from pydantic import BaseModel, Field, ValidationError

app = FastAPI()


class Pedido(BaseModel):
    numero: int = Field(gt=0)
    cliente: str = Field(min_length=3)


@app.post("/pedidos", response_class=HTMLResponse)
def crear_pedido(
    numero: str = Form(),
    cliente: str = Form(),
):
    try:
        pedido = Pedido(numero=numero, cliente=cliente)
    except ValidationError:
        return HTMLResponse(
            '<p class="error">Revisa el número y el nombre del cliente.</p>',
            status_code=422,
        )

    return HTMLResponse(
        f'<p>Pedido {pedido.numero} registrado.</p>'
    )
```

## HTML

```html
<form hx-post="/pedidos" hx-target="#resultado">
    <label>
        Número
        <input name="numero" type="number" required>
    </label>

    <label>
        Cliente
        <input name="cliente" required>
    </label>

    <button type="submit">Registrar</button>
    <div id="resultado"></div>
</form>
```

## Reglas operativas

- Devuelve HTML con los mensajes de error.
    
- Usa `hx-target` para mostrar la respuesta dentro del formulario.
    
- Devuelve `422` cuando los datos sean inválidos.
    
- En HTMX 4, configura explícitamente el manejo de respuestas HTTP de error si necesitas intercambiar el HTML de un `422`; no asumas que se insertará automáticamente.
    
- Conserva los valores enviados para evitar que el usuario tenga que rellenar el formulario de nuevo.
    

**Principio:** valida en el servidor, devuelve errores comprensibles y permite corregirlos en el mismo formulario.