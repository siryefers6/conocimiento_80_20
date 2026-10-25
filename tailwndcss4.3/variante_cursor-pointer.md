# Tailwind CSS: `cursor-pointer`

Indica visualmente que un elemento es interactivo mediante el cursor de mano. Mejora la percepción de botones, enlaces y controles clicables.

## Ejemplo funcional

```html
<button
    class="rounded-md bg-blue-600 px-4 py-2 text-white cursor-pointer"
>
    Guardar
</button>
```

## Clases relacionadas

|Clase|Función|
|---|---|
|`cursor-pointer`|Indica que el elemento puede activarse|
|`cursor-not-allowed`|Indica que una acción no está disponible|
|`cursor-wait`|Indica que se está esperando|
|`cursor-default`|Usa el cursor predeterminado|

## Regla esencial

`cursor-pointer` solo cambia la apariencia del cursor. No convierte un elemento en un botón ni añade funcionalidad.

Usa `<button>` para acciones y `<a>` para navegación. Aplica `disabled` a botones deshabilitados.

**Principio:** el cursor comunica interactividad; el HTML define el comportamiento.