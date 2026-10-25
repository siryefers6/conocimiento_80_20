# Página completa vs. fragmento HTML

## Página completa

Devuelve el documento HTML entero, incluyendo `html`, `head` y `body`. Úsala para cargar una página nueva.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Pedidos</title>
</head>
<body>
    <h1>Pedidos</h1>
    <div id="lista">...</div>
</body>
</html>
```

## Fragmento HTML

Devuelve únicamente el contenido que necesitas insertar o reemplazar. Úsalo para actualizar una sección sin recargar toda la página.

```html
<div id="lista">
    <p>Pedido #123</p>
</div>
```

## Ejemplo con FastAPI y HTMX

Backend — `main.py`

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()@app.get("/pedidos", response_class=HTMLResponse)
def pedidos():
	return """
	<div id="lista">
		<p>Pedido #123</p>
	</div>
	"""
```

HTML

```html
<button hx-get="/pedidos" hx-target="#lista">
    Actualizar pedidos
</button>

<div id="lista"></div>
```

HTMX inserta la respuesta dentro de `#lista`. No reemplaza toda la página.

## Regla operativa

- Navegación: devuelve una página completa.
    
- Interacción HTMX: devuelve un fragmento HTML dirigido al elemento objetivo.
    
- Consistencia: el fragmento debe coincidir con la estructura que espera `hx-target` y el método `hx-swap`.
    

Nota: el ejemplo ilustra el patrón general; adapta la respuesta a la estrategia de intercambio configurada en tu versión de HTMX.