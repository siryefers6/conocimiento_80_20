# HTMX 4: enviar datos sin recargar la página

Usa `hx-post` para enviar datos al servidor y actualizar una sección del DOM sin recargar el documento completo.

## Ejemplo con FastAPI

**Backend —** `main.py`

```python
from fastapi import FastAPI, Form
app = FastAPI()

@app.post("/saludar", response_class=HTMLResponse)
def saludar(nombre: str = Form()):
    return f"<p>Hola, {nombre}.</p>"
```

**HTML**

```html
<form hx-post="/saludar" hx-target="#resultado">
    <input name="nombre" required>
    <button type="submit">Enviar</button>
</form>

<div id="resultado"></div>
```

## Flujo

1. El usuario completa el formulario.
    
2. HTMX envía los datos mediante `POST`.
    
3. FastAPI procesa la petición y devuelve HTML.
    
4. HTMX inserta la respuesta en `#resultado`.
    
5. El documento permanece cargado; no hay una recarga completa.
    

## Reglas operativas

- Usa `hx-post` para enviar datos.
    
- Usa `hx-target` para seleccionar dónde mostrar la respuesta.
    
- Devuelve HTML cuando la respuesta deba actualizar directamente la interfaz.
    
- Valida los datos en FastAPI.
    

**Principio:** actualiza solo el componente necesario, no toda la página.