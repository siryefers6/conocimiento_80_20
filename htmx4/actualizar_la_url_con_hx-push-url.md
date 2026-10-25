# HTMX 4: actualizar la URL con `hx-push-url`

Usa `hx-push-url` para añadir una entrada al historial del navegador cuando HTMX realiza una petición. Permite compartir la URL y navegar con los botones Atrás y Adelante.

## Ejemplo: filtrar pedidos

```html
<form hx-get="/pedidos"
      hx-target="#resultados"
      hx-swap="innerHTML"
      hx-push-url="true">
    <input name="estado" placeholder="Estado del pedido">
    <button type="submit">Filtrar</button>
</form>

<div id="resultados"></div>
```

Al enviar el formulario, HTMX actualiza los resultados y añade la URL de la petición al historial, incluidos sus parámetros de consulta.

## Valores disponibles

- `hx-push-url="true"`: añade al historial la URL de la petición.
    
- `hx-push-url="/pedidos?estado=pendiente"`: añade una URL específica.
    
- `hx-push-url="false"`: evita añadir una entrada al historial para esa petición.
    

## Diferencia con `hx-replace-url`

`hx-push-url` crea una nueva entrada en el historial; `hx-replace-url` sustituye la entrada actual.

## Reglas esenciales

- Usa `hx-push-url` para búsquedas, filtros y navegación que deban conservarse al volver atrás.
    
- Asegúrate de que la URL resultante pueda cargar el estado correspondiente mediante una navegación normal.
    
- No confundas actualizar la URL con guardar los datos: el servidor debe procesar los parámetros y reconstruir la vista.
    

**Principio:** sincroniza la URL con el estado visible para permitir navegación y enlaces compartibles.