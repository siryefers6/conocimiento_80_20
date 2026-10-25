Entrega datos a una plantilla mediante `context`. Jinja2 utiliza esos valores para generar HTML dinámico.

## 1. Enviar datos desde FastAPI

`main.py`

```python
from fastapi import FastAPI, Request
from fastapi.templating import Jinja2Templates

app = FastAPI()
templates = Jinja2Templates(directory="templates")


@app.get("/pedidos")
def pedidos(request: Request):
    datos = {
        "titulo": "Lista de pedidos",
        "pedidos": [101, 102, 103],
    }

    return templates.TemplateResponse(
        request=request,
        name="pedidos.html",
        context=datos,
    )
```

## 2. Recibir datos en Jinja2

`templates/pedidos.html`

```html
<h1>{{ titulo }}</h1>

<ul>
    {% for pedido in pedidos %}
        <li>Pedido #{{ pedido }}</li>
    {% endfor %}
</ul>
```

## Conceptos esenciales

| Elemento       | Función                               |
| -------------- | ------------------------------------- |
| `context`      | Diccionario que transporta los datos. |
| `{{ titulo }}` | Muestra el valor de una variable.     |
| `{% for %}`    | Recorre una colección.                |
| `{{ pedido }}` | Muestra el elemento actual.           |

## Regla operativa

- Pasa los datos desde FastAPI mediante `context`.
    
- Usa diccionarios para agrupar valores relacionados.
    
- Usa `{{ }}` para mostrar datos y `{% %}` para controlar la plantilla.
    
- Mantén la lógica de negocio en Python; limita Jinja2 a presentar los datos.
    

Principio: FastAPI prepara los datos; Jinja2 los transforma en HTML.