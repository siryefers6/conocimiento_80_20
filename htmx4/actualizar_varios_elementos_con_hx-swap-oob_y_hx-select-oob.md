# HTMX 4: actualizar varios elementos con `hx-swap-oob` y `hx-select-oob`

Actualiza varios componentes con una sola respuesta del servidor. El elemento principal actualiza el destino normal; los elementos adicionales actualizan destinos específicos.

## `hx-swap-oob`: enviar componentes adicionales

Añade `hx-swap-oob` al elemento HTML que quieras actualizar fuera del destino principal.

**HTML**

```html
<button hx-post="/pedidos/1/confirmar"
        hx-target="#pedido-1"
        hx-swap="outerHTML">
    Confirmar
</button>

<div id="pedido-1">Pendiente</div>
<span id="contador">0</span>
```

**Respuesta del servidor**

```html
<div id="pedido-1">Confirmado</div>

<span id="contador" hx-swap-oob="true">1</span>
```

HTMX reemplaza `#pedido-1` con el contenido principal y actualiza `#contador` mediante el intercambio adicional (_out-of-band swap_).

## `hx-select-oob`: seleccionar elementos adicionales

Usa `hx-select-oob` en la solicitud para extraer elementos concretos de la respuesta y actualizarlos en otros destinos.

**HTML**

```html
<button hx-get="/dashboard"
        hx-target="#main"
        hx-swap="outerHTML"
        hx-select="#main"
        hx-select-oob="#alert,#contador">
    Actualizar panel
</button>

<div id="main">Contenido anterior</div>
<div id="alert">Sin alertas</div>
<span id="contador">0</span>
```

**Respuesta del servidor**

```html
<div id="main">Contenido actualizado</div>
<div id="alert">Datos actualizados</div>
<span id="contador">5</span>
```

HTMX selecciona `#main` para el intercambio principal y actualiza también `#alert` y `#contador`.

## Diferencias esenciales

| Atributo        | Se configura en                   | Función                                             |
| --------------- | --------------------------------- | --------------------------------------------------- |
| `hx-swap-oob`   | Respuesta HTML                    | Marca elementos adicionales que deben actualizarse. |
| `hx-select-oob` | Elemento que realiza la solicitud | Selecciona elementos adicionales de la respuesta.   |

## Regla de decisión

- Usa `hx-swap-oob` cuando quieras marcar explícitamente las actualizaciones desde el HTML generado por el servidor.
    
- Usa `hx-select-oob` cuando quieras seleccionar los elementos adicionales desde el HTML que realiza la solicitud.
    
- Ambos requieren que los elementos destino puedan identificarse por su `id`.
    

Consulta la documentación oficial de HTMX 4: [hx-swap-oob](https://four.htmx.org/reference/attributes/hx-swap-oob/) y [hx-select-oob](https://four.htmx.org/reference/attributes/hx-select-oob/).