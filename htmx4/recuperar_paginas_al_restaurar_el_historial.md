# HTMX 4: recuperar páginas al restaurar el historial

HTMX 4 vuelve a solicitar la página al servidor cuando el usuario navega hacia atrás o adelante, en lugar de depender de una copia guardada automáticamente en `localStorage`.

## Flujo de restauración

1. El usuario navega a `/pedidos?pagina=2`.
    
2. `hx-push-url` registra la URL en el historial.
    
3. El usuario visita otra página.
    
4. Pulsa **Atrás**.
    
5. HTMX solicita al servidor la URL anterior y restaura su contenido.
    

## Ejemplo con FastAPI

```python
@app.get("/pedidos", response_class=HTMLResponse)
def listar_pedidos(request: Request, pagina: int = 1):
    pedidos = obtener_pedidos(pagina)

    plantilla = (
        "fragments/pedidos.html"
        if request.headers.get("HX-Request") == "true"
        else "pedidos.html"
    )

    return templates.TemplateResponse(
        request=request,
        name=plantilla,
        context={"pedidos": pedidos, "pagina": pagina},
    )
```

La misma ruta reconstruye la vista usando los parámetros de la URL. Para una navegación normal devuelve la página completa; para una petición HTMX devuelve el fragmento solicitado.

## Consideraciones

- **Datos actualizados:** el servidor puede devolver información más reciente que la mostrada originalmente.
    
- **URL reproducible:** los filtros y la paginación deben estar en los parámetros de consulta.
    
- **Autenticación:** vuelve a validar permisos en cada petición.
    
- **Caché del navegador:** puede intervenir en las solicitudes HTTP; no confundas esa caché con el historial de HTMX.
    

## Regla esencial

No dependas de que HTMX conserve el HTML anterior en memoria. Diseña las rutas para reconstruir la vista a partir de la URL y de los datos actuales del servidor.

**Principio:** restaurar el historial significa recuperar el estado navegable, no necesariamente reproducir una fotografía exacta de la pantalla anterior.