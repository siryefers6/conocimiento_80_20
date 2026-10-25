# Comunicación HTTP desde HTML con HTMX

Envía peticiones HTTP desde elementos HTML mediante atributos `hx-*`. Evita JavaScript para las interacciones habituales.

## Métodos HTTP principales

|Atributo|Uso|
|---|---|
|`hx-get`|Consultar datos.|
|`hx-post`|Crear o enviar datos.|
|`hx-put`|Actualizar un recurso.|
|`hx-delete`|Eliminar un recurso.|

## Ejemplo mínimo con FastAPI

Backend — `main.py`

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()@app.get("/saludo", response_class=HTMLResponse)
def saludo():
	return "<p>Petición GET completada</p>"
```

HTML

```html
<button hx-get="/saludo" hx-target="#resultado">
    Consultar
</button>

<div id="resultado"></div>
```

Al pulsar el botón:

1. HTMX envía una petición `GET /saludo`.
    
2. FastAPI procesa la petición.
    
3. El servidor devuelve HTML.
    
4. HTMX inserta la respuesta en `#resultado`.
    

## Conceptos esenciales

- Petición: solicitud enviada al servidor.
    
- Respuesta: contenido y estado HTTP devueltos.
    
- Endpoint: URL que procesa la petición.
    
- Fragmento HTML: parte de una página que el servidor devuelve para actualizar la interfaz.
    

Regla: usa `GET` para consultar y `POST` para enviar datos o ejecutar operaciones que modifican el estado. Usa `PUT` y `DELETE` cuando el diseño de tu API lo requiera.

Nota de versión: verifica la sintaxis con HTMX 4.0.0 antes de reutilizar ejemplos de versiones anteriores.