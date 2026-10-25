# FastAPI: validar formularios con Pydantic

Valida los datos en el servidor antes de procesarlos. Usa Pydantic para definir tipos, restricciones y reglas de validación.

## Ejemplo con FastAPI y HTMX 4

`**main.py**`

```python
from fastapi import FastAPI, Form
from fastapi.responses import HTMLResponse
from pydantic import BaseModel, Field, ValidationError

app = FastAPI()


class Pedido(BaseModel):
    numero: int = Field(gt=0)
    cliente: str = Field(min_length=3, max_length=50)


@app.post("/pedidos", response_class=HTMLResponse)
def crear_pedido(
    numero: int = Form(),
    cliente: str = Form(),
):
    try:
        pedido = Pedido(numero=numero, cliente=cliente)
    except ValidationError:
        return HTMLResponse(
            "<p>Revisa los datos ingresados.</p>",
            status_code=422,
        )

    return HTMLResponse(
        f"<p>Pedido {pedido.numero} registrado para {pedido.cliente}.</p>"
    )
```

**HTML**

```html
<form hx-post="/pedidos" hx-target="#resultado">
    <input name="numero" type="number" min="1" required>
    <input name="cliente" minlength="3" maxlength="50" required>
    <button type="submit">Registrar</button>
</form>

<div id="resultado"></div>
```

## Reglas operativas

- `BaseModel`: define la estructura de los datos.
    
- `Field(gt=0)`: exige un número mayor que cero.
    
- `Field(min_length=3)`: establece una longitud mínima.
    
- `ValidationError`: captura los errores de validación de Pydantic.
    
- `422`: comunica que los datos no cumplen las reglas.
    

**Principio:** el navegador facilita la entrada; Pydantic garantiza la validación en el servidor.