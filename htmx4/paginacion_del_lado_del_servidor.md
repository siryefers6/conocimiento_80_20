# FastAPI + HTMX 4: paginación del lado del servidor

Divide los resultados en páginas. FastAPI consulta únicamente los registros necesarios; HTMX actualiza la tabla y los controles de navegación sin recargar la página.

## 1. Crear la tabla y el contenedor

`templates/pedidos.html`

```html
<div id="tabla-pedidos"
     hx-get="/pedidos"
     hx-trigger="load"
     hx-swap="innerHTML">
    Cargando pedidos...
</div>
```

## 2. Consultar una página con SQLModel

```python
@app.get("/pedidos", response_class=HTMLResponse)
def listar_pedidos(
    request: Request,
    pagina: int = 1,
    limite: int = 10,
):
    pagina = max(1, pagina)
    limite = min(max(1, limite), 100)

    with Session(engine) as session:
        total = session.exec(
            select(func.count()).select_from(Pedido)
        ).one()

        pedidos = session.exec(
            select(Pedido)
            .order_by(Pedido.id)
            .offset((pagina - 1) * limite)
            .limit(limite)
        ).all()

    return templates.TemplateResponse(
        request=request,
        name="fragments/tabla_pedidos.html",
        context={
            "pedidos": pedidos,
            "pagina": pagina,
            "limite": limite,
            "total_paginas": max(1, (total + limite - 1) // limite),
        },
    )
```

Importa `select` y `func` desde `sqlmodel`, además de `Session`.

- `offset()`: omite los registros de páginas anteriores.
    
- `limit()`: limita los registros consultados.
    
- `count()`: obtiene el total para calcular las páginas.
    

## 3. Renderizar las filas y los controles

`templates/fragments/tabla_pedidos.html`

```html
<table>
    <thead>
        <tr>
            <th>Número</th>
            <th>Estado</th>
        </tr>
    </thead>
    <tbody>
        {% for pedido in pedidos %}
        <tr>
            <td>{{ pedido.numero }}</td>
            <td>{{ pedido.estado }}</td>
        </tr>
        {% endfor %}
    </tbody>
</table>

<nav>
    {% if pagina > 1 %}
    <button hx-get="/pedidos?pagina={{ pagina - 1 }}&limite={{ limite }}"
            hx-target="#tabla-pedidos"
            hx-swap="innerHTML">
        Anterior
    </button>
    {% endif %}

    <span>Página {{ pagina }} de {{ total_paginas }}</span>

    {% if pagina < total_paginas %}
    <button hx-get="/pedidos?pagina={{ pagina + 1 }}&limite={{ limite }}"
            hx-target="#tabla-pedidos"
            hx-swap="innerHTML">
        Siguiente
    </button>
    {% endif %}
</nav>
```

## Reglas esenciales

- Valida `pagina` y limita el tamaño máximo de página.
    
- Aplica `ORDER BY` para mantener un orden consistente.
    
- Ejecuta el conteo y la consulta de registros en el servidor.
    
- Mantén la tabla y los botones dentro del mismo contenedor actualizado.
    

**Principio:** `OFFSET` determina dónde comenzar; `LIMIT` determina cuántos registros devolver. HTMX reemplaza el fragmento, no la página completa.