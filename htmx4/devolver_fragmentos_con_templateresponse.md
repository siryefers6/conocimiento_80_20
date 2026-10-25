Renderiza una plantilla Jinja2 y devuelve únicamente el HTML necesario para actualizar una sección mediante HTMX 4.

## Estructura

```
proyecto/
├── main.py
└── templates/
    └── fragments/
        └── pedidos.html
```

## 1. Crear el endpoint

`main.py`

```python
from fastapi import FastAPI, Request
from fastapi.templating import Jinja2Templates

app = FastAPI()
templates = Jinja2Templates(directory="templates")


@app.get("/pedidos")
def obtener_pedidos(request: Request):
    pedidos = [101, 102, 103]

    return templates.TemplateResponse(
        request=request,
        name="fragments/pedidos.html",
        context={"pedidos": pedidos},
    )
```

## 2. Crear el fragmento

`templates/fragments/pedidos.html`

```html
<ul>
    {% for pedido in pedidos %}
        <li>Pedido #{{ pedido }}</li>
    {% endfor %}
</ul>
```

## 3. Solicitar el fragmento con HTMX 4

```html
<button hx-get="/pedidos" hx-target="#resultado">
    Consultar pedidos
</button>

<div id="resultado"></div>
```

Al pulsar el botón, FastAPI renderiza la plantilla y devuelve el HTML. HTMX lo inserta dentro de `#resultado`.

## Reglas operativas

- Usa `Jinja2Templates` para gestionar las plantillas.
    
- Pasa `request`, el nombre de la plantilla y el contexto a `TemplateResponse`.
    
- Guarda los fragmentos en `templates/fragments/` para distinguirlos de las páginas completas.
    
- Devuelve solo el HTML que debe actualizarse.
    

Principio: Jinja2 genera el fragmento; FastAPI lo entrega; HTMX actualiza el DOM.