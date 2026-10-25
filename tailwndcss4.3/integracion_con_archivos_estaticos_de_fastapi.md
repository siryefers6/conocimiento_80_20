# Tailwind CSS: Integración con archivos estáticos de FastAPI

Tailwind CSS genera un archivo CSS estático. FastAPI lo sirve mediante `StaticFiles` y Jinja2 lo incorpora en las plantillas HTML.

## 1. Montar archivos estáticos

En `app/main.py`:

```python
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles

app = FastAPI()

app.mount(
    "/static",
    StaticFiles(directory="static"),
    name="static",
)
```

## 2. Vincular el CSS en Jinja2

En `templates/base.html`:

```html
<link
    rel="stylesheet"
    href="{{ url_for('static', path='/css/output.css') }}"
>
```

`url_for()` genera la URL correspondiente a la ruta estática registrada en FastAPI.

## 3. Estructura del proyecto

```
proyecto/
├── app/
│   └── main.py
├── static/
│   └── css/
│       └── output.css
└── templates/
    └── base.html
```

## 4. Flujo de funcionamiento

1. Tailwind compila `src/input.css`.
    
2. Genera `static/css/output.css`.
    
3. FastAPI expone el directorio `static/` mediante `/static`.
    
4. Jinja2 incorpora el CSS mediante `url_for()`.
    
5. El navegador solicita el archivo y aplica los estilos.
    

## Consideraciones

- Ruta física: `static/css/output.css`.
    
- URL pública: `/static/css/output.css`.
    
- Compilación: ejecuta Tailwind nuevamente cuando cambien las plantillas o los estilos.
    
- Producción: genera el CSS antes de desplegar la aplicación.
    

## Regla esencial

Distingue la ruta física del archivo de su URL pública. FastAPI conecta ambas mediante `StaticFiles`.

Principio: Tailwind genera los estilos; FastAPI los sirve; Jinja2 los enlaza.