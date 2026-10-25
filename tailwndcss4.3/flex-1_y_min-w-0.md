# Tailwind CSS: `flex-1` y `min-w-0`

Controla cómo crecen los elementos Flexbox y evita que su contenido fuerce el ancho del contenedor.

## Clases esenciales

| Clase     | Función                                                                                   |
| --------- | ----------------------------------------------------------------------------------------- |
| `flex-1`  | Permite que el elemento crezca y ocupe el espacio disponible                              |
| `min-w-0` | Permite que el elemento se reduzca por debajo del ancho mínimo automático de su contenido |

## Ejemplo funcional

```html
<div class="flex gap-4">
    <aside class="w-48 shrink-0 bg-gray-100 p-4">
        Menú
    </aside>

    <main class="min-w-0 flex-1">
        <p class="truncate">
            Texto muy largo que no debe desbordar el contenedor...
        </p>
    </main>
</div>
```

- `flex-1`: asigna al contenido principal el espacio restante.
    
- `shrink-0`: impide que el menú reduzca su ancho.
    
- `min-w-0`: permite que el contenido principal se contraiga.
    
- `truncate`: recorta el texto con puntos suspensivos cuando no cabe.
    

## ¿Por qué usar `min-w-0`?

Los elementos Flexbox tienen normalmente `min-width: auto`. Esto puede impedir que se reduzcan cuando contienen texto largo, tablas o elementos anchos.

Aplica `min-w-0` al elemento flexible que debe poder contraerse.

## Regla esencial

Combina `flex-1 min-w-0` cuando un elemento deba ocupar el espacio restante sin provocar desbordamientos.

**Principio:** permite crecer con `flex-1`; permite contraerse con `min-w-0`.