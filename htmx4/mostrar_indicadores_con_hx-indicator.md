# HTMX 4: mostrar indicadores con `hx-indicator`

Usa `hx-indicator` para mostrar un elemento mientras HTMX procesa una petición. Indica al usuario que la operación está en curso.

## Ejemplo funcional

```html
<button hx-post="/pedidos"
        hx-indicator="#cargando">
    Guardar pedido
</button>

<span id="cargando" class="htmx-indicator">
    Guardando...
</span>
```

```js
.htmx-indicator {
    display: none;
}

.htmx-request .htmx-indicator,
.htmx-request.htmx-indicator {
    display: inline;
}
```

HTMX activa la clase `htmx-request` durante la petición y la retira al terminar. El indicador aparece y desaparece automáticamente.

## Mostrar un indicador en un contenedor

```html
<div id="resultados">
    <span id="cargando" class="htmx-indicator">
        Cargando resultados...
    </span>
</div>

<button hx-get="/pedidos"
        hx-target="#resultados"
        hx-indicator="#cargando">
    Consultar
</button>
```

## Reglas esenciales

- `hx-indicator` recibe un selector CSS.
    
- `htmx-indicator` define el elemento que se mostrará durante la petición.
    
- No necesitas JavaScript para controlar su visibilidad.
    
- Usa indicadores para búsquedas, formularios y operaciones que puedan tardar.
    

**Principio:** comunica que la petición está en curso sin bloquear innecesariamente toda la interfaz.