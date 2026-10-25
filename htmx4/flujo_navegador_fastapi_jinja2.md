# Flujo navegador → FastAPI → Jinja2

## Responsabilidades

- Navegador: solicita páginas y muestra HTML.
    
- FastAPI: recibe peticiones HTTP, ejecuta la lógica y entrega respuestas.
    
- Jinja2: genera HTML a partir de plantillas y datos.
    

## Flujo de una petición

1. El navegador solicita una URL.
    
2. FastAPI recibe la petición y ejecuta el endpoint.
    
3. El endpoint obtiene los datos necesarios.
    
4. Jinja2 combina la plantilla con esos datos.
    
5. FastAPI devuelve el HTML generado.
    
6. El navegador interpreta y muestra el resultado.
    

## Ejemplo mínimo funcional

Estructura

```
proyecto/
├── main.py
└── templates/
    └── pedidos.html
```

`main.py`

```python
from fastapi import FastAPI, Request
from fastapi.templating import Jinja2Templates

app = FastAPI()

templates = Jinja2Templates(directory="templates")

@app.get("/pedidos")
def pedidos(request: Request):
	datos = {"pedidos": [101, 102, 103]}
	
	return templates.TemplateResponse(
		request=request,
		name="pedidos.html",
		context=datos,
	)
```

`templates/pedidos.html`

```html
<h1>Pedidos</h1>

<ul>
    {% for pedido in pedidos %}
        <li>Pedido {{ pedido }}</li>
    {% endfor %}
</ul>
```

Ejecuta:

```
uvicorn main:app --reload
```

Abre `http://127.0.0.1:8000/pedidos`.

## Regla fundamental

FastAPI procesa; Jinja2 renderiza; el navegador muestra.

Con HTMX, el flujo es el mismo, pero el navegador puede solicitar y reemplazar únicamente un fragmento HTML en lugar de cargar una página completa.