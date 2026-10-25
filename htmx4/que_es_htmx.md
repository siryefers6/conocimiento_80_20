## ¿Qué es HTMX?

HTMX permite crear interfaces web dinámicas usando atributos HTML, sin escribir JavaScript para las interacciones habituales.

Principio: HTML solicita; el servidor procesa; HTML actualiza la interfaz.

## ¿Cómo funciona?

1. Define una acción mediante atributos `hx-*`.
    
2. HTMX envía una petición HTTP al servidor.
    
3. FastAPI procesa la petición y devuelve HTML.
    
4. HTMX reemplaza una parte del DOM con la respuesta.
    

## Ejemplo mínimo

FastAPI — `main.py`

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.get("/saludo", response_class=HTMLResponse)
def saludo():
	return "<p>Respuesta del servidor</p>"
```

HTML

```html
<button hx-get="/saludo" hx-target="#resultado">
    Saludar
</button>

<div id="resultado"></div>

<script src="https://unpkg.com/htmx.org@2.0.8"></script>
```

Al pulsar el botón, HTMX solicita `/saludo` y coloca el HTML recibido dentro de `#resultado`.

> HTMX 4: verifica la versión instalada antes de copiar los atributos del ejemplo. La versión 4 puede tener diferencias respecto de HTMX 2.

## Atributos esenciales

| Atributo     | Función                               |
| ------------ | ------------------------------------- |
| `hx-get`     | Envía una petición GET.               |
| `hx-post`    | Envía una petición POST.              |
| `hx-target`  | Define dónde insertar la respuesta.   |
| `hx-swap`    | Define cómo reemplazar el contenido.  |
| `hx-trigger` | Define qué evento inicia la petición. |

## Regla fundamental

Devuelve fragmentos HTML desde FastAPI cuando solo necesites actualizar una sección. Reserva las respuestas JSON para APIs que realmente las necesiten.