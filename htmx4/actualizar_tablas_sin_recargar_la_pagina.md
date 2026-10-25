# HTMX 4: actualizar tablas sin recargar la página

Actualiza las filas de una tabla mediante una petición HTTP que devuelve HTML parcial. Conserva el resto de la página intacto.

## 1. Crear la tabla

`templates/pedidos.html`

```html
<button hx-get="/pedidos/filas"
        hx-target="#filas-pedidos"
        hx-swap="innerHTML">
    Actualizar tabla
</button>

<table>
    <thead>
        <tr>
            <th>Número</th>
            <th>Estado</th>
        </tr>
    </thead>
    <tbody id="filas-pedidos">
        {% include "fragments/filas_pedidos.html" %}
    </tbody>
</table>
```

## 2. Devolver las filas desde FastAPI

```python
@app.get("/pedidos/filas", response_class=HTMLResponse)
def listar_filas(request: Request):
    pedidos = obtener_pedidos()

    return templates.TemplateResponse(
        request=request,
        name="fragments/filas_pedidos.html",
        context={"pedidos": pedidos},
    )
```

`obtener_pedidos()` representa la consulta a la base de datos.

## 3. Crear el fragmento HTML

`templates/fragments/filas_pedidos.html`

```html
{% for pedido in pedidos %}
<tr>
    <td>{{ pedido.numero }}</td>
    <td>{{ pedido.estado }}</td>
</tr>
{% endfor %}
```

## Cómo funciona

1. El usuario pulsa **Actualizar tabla**.
    
2. HTMX solicita las filas a FastAPI.
    
3. FastAPI consulta los datos y renderiza el fragmento.
    
4. HTMX reemplaza el contenido de `#filas-pedidos`.
    

## Reglas esenciales

- Usa `hx-target="#filas-pedidos"` para seleccionar el `<tbody>`.
    
- Usa `hx-swap="innerHTML"` para reemplazar sus filas sin eliminar el contenedor.
    
- Devuelve únicamente las filas `<tr>`, no otra tabla completa.
    
- Para agregar una fila sin reemplazar las existentes, usa `hx-swap="beforeend"`.
    

**Principio:** actualiza el fragmento más pequeño que contenga los datos modificados.