# Tailwind CSS: Diseño adaptable (responsive)

Aplica el enfoque **mobile-first**: define primero los estilos para móviles y añade variantes para pantallas más grandes.

## Variantes responsive

| Variante | Ancho mínimo predeterminado |
| -------- | --------------------------- |
| `sm:`    | 640 px                      |
| `md:`    | 768 px                      |
| `lg:`    | 1024 px                     |
| `xl:`    | 1280 px                     |
| `2xl:`   | 1536 px                     |

## Ejemplo: tarjetas adaptables

```html
<div class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3">
    <article class="rounded-lg bg-white p-4 shadow-md">
        Pedido 1
    </article>
    <article class="rounded-lg bg-white p-4 shadow-md">
        Pedido 2
    </article>
    <article class="rounded-lg bg-white p-4 shadow-md">
        Pedido 3
    </article>
</div>
```

- Móvil: una columna.
    
- Desde `md`: dos columnas.
    
- Desde `lg`: tres columnas.
    

## Ejemplo: formulario adaptable

```html
<form class="mx-auto w-full max-w-xl space-y-4 p-4">
    <input class="w-full rounded-lg border p-2" placeholder="Nombre">

    <div class="flex flex-col gap-2 sm:flex-row">
        <button class="w-full rounded-lg bg-blue-600 p-2 text-white sm:w-auto">
            Guardar
        </button>
        <button class="w-full rounded-lg bg-gray-200 p-2 sm:w-auto">
            Cancelar
        </button>
    </div>
</form>
```

En móviles, los botones se apilan. Desde `sm`, se colocan en una fila.

## Reglas esenciales

- **Mobile-first:** define primero las clases sin prefijo.
    
- **Variantes:** usa `md:` o `lg:` para sobrescribir estilos en pantallas mayores.
    
- **Ancho:** combina `w-full` con `max-w-*` para evitar formularios excesivamente anchos.
    
- **Tablas:** evita asumir que todas las columnas caben en móviles; considera desplazamiento horizontal con `overflow-x-auto`.
    

**Principio:** construye primero para la pantalla pequeña. Amplía el diseño solo cuando el espacio disponible lo permita.