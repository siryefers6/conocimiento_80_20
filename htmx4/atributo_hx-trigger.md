# HTMX: atributo `hx-trigger`

Define el evento que inicia una petición HTTP. Controla cuándo HTMX consulta o envía datos al servidor.

## Eventos esenciales

| Valor    | Ejecución                        |
| -------- | -------------------------------- |
| `click`  | Al hacer clic.                   |
| `submit` | Al enviar un formulario.         |
| `keyup`  | Al soltar una tecla.             |
| `change` | Al cambiar el valor de un campo. |
| `load`   | Al cargar el elemento.           |

## Ejemplo: búsqueda automática

FastAPI — `main.py`

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.get("/buscar", response_class=HTMLResponse)
def buscar(q: str = ""):
    return f"<p>Buscando: {q}</p>"
```

HTML

```html
<input
    name="q"
    hx-get="/buscar"
    hx-trigger="keyup changed delay:500ms"
    hx-target="#resultado"
>

<div id="resultado"></div>
```

Cada vez que cambie el texto, HTMX espera 500 ms sin nuevas pulsaciones antes de enviar la petición.

## Modificadores útiles

- `changed`: ejecuta solo si cambió el valor.
    
- `delay:500ms`: espera antes de ejecutar.
    
- `once`: ejecuta una sola vez.
    
- `every 5s`: repite el evento cada cinco segundos.
    

## Regla operativa

Usa `hx-trigger` para controlar el momento de ejecución. Combínalo con `hx-get` o `hx-post` según la operación.

Nota: este ejemplo usa sintaxis habitual de HTMX 2. Verifica los modificadores disponibles en HTMX 4.0.0 antes de aplicarlos.