# HTMX 4: `hx-boost` y navegación progresiva

Usa `hx-boost="true"` para convertir enlaces y formularios HTML tradicionales en peticiones AJAX, evitando recargas completas cuando HTMX puede procesarlas.

## Ejemplo con FastAPI y Jinja2

**Plantilla base**

```html
<nav hx-boost="true">
    <a href="/">Inicio</a>
    <a href="/pedidos">Pedidos</a>
</nav>

<main id="contenido">
    {% block contenido %}{% endblock %}
</main>
```

Los enlaces dentro de la navegación se procesan mediante HTMX. La respuesta se intercambia en el destino predeterminado, normalmente el `<body>`.

## Elegir el destino

Para actualizar solamente una sección, configura explícitamente el destino y el intercambio:

```html
<nav>
    <a href="/pedidos"
       hx-boost="true"
       hx-target="#contenido"
       hx-swap="innerHTML">
        Pedidos
    </a>
</nav>

<main id="contenido">
    Contenido actual
</main>
```

La ruta debe devolver el HTML apropiado para el intercambio solicitado.

## Navegación progresiva

- Mantén enlaces con `href` y formularios HTML funcionales.
    
- Sin JavaScript, el navegador puede navegar mediante solicitudes tradicionales.
    
- Con HTMX, la navegación se actualiza mediante peticiones asíncronas.
    
- Comprueba que las rutas puedan devolver una página completa cuando se solicite una navegación normal.
    

## Regla esencial

Usa `hx-boost` para mejorar la navegación existente sin convertirla en una aplicación dependiente de JavaScript.

**Principio:** HTML funcional primero; HTMX mejora la experiencia.