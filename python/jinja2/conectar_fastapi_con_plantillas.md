## 1. `Jinja2Templates`

Objetivo: conectar FastAPI con las plantillas Jinja2.

```python
from fastapi.templating import Jinja2Templates

templates = Jinja2Templates(directory="templates")
```

- `Jinja2Templates`: configura el directorio de plantillas.
    
- `directory`: carpeta donde guardas los archivos HTML.
    

## 2. `TemplateResponse`

Objetivo: devolver HTML desde una ruta de FastAPI.

```python
@app.get("/")
def inicio(request: Request):
	return templates.TemplateResponse(
	request=request,
	name="index.html",
	context={"nombre": "Abathur"},
	)
```

- `request`: objeto de la solicitud HTTP.
    
- `name`: nombre de la plantilla.
    
- `context`: datos que recibe Jinja2.
    

## 3. `Contexto Jinja2`

Objetivo: enviar variables, listas, diccionarios y objetos a HTML.

```python
context = {
		"nombre": "Abathur",
		"usuarios": ["Kerrigan", "Raynor"],
		"pedido": {"numero": 123, "estado": "En curso"},
}
```

Jinja2:

```html
<p>{{ nombre }}</p>

{% for usuario in usuarios %}
    <p>{{ usuario }}</p>
{% endfor %}

<p>{{ pedido.numero }}</p>
<p>{{ pedido["estado"] }}</p>
```

- Variables: `{{ nombre }}`
    
- Listas: `{% for item in lista %}`
    
- Diccionarios: `{{ pedido.numero }}`
    
- Objetos: `{{ objeto.atributo }}`
    

## 4. `FastAPI HTML vs JSON.md`

Objetivo: distinguir páginas web de endpoints de API.

| Característica | HTML con Jinja2      | API JSON                          |
| -------------- | -------------------- | --------------------------------- |
| Respuesta      | HTML                 | JSON                              |
| Uso            | Navegador y páginas  | Aplicaciones y clientes           |
| Retorno        | `TemplateResponse()` | `dict`, listas o modelos Pydantic |

```python
@app.get("/web")
def web(request: Request):
	return templates.TemplateResponse(
		request=request,
		name="index.html",
		context={"nombre": "Abathur"},
	)

@app.get("/api")
def api():
	return {"nombre": "Abathur"}
```