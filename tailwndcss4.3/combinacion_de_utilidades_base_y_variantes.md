# Tailwind CSS: Combinación de utilidades base y variantes

Combina clases sin prefijo con variantes responsive o de estado para modificar estilos según el tamaño de pantalla o la interacción del usuario.

## 1. Utilidades base

Define el comportamiento predeterminado, pensado primero para móviles.

```html
<div class="grid grid-cols-1 gap-4">
    ...
</div>
```

## 2. Variantes responsive

Añade prefijos para modificar el comportamiento desde un ancho mínimo.

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

- `grid-cols-1`: una columna por defecto.
    
- `md:grid-cols-2`: dos columnas desde 768 px.
    
- `lg:grid-cols-3`: tres columnas desde 1024 px.
    

## 3. Combinar variantes de estado

Usa `hover:` para cambiar el estilo al pasar el cursor y `focus:` para destacar un elemento enfocado.

```html
<button class="rounded-lg bg-blue-600 px-4 py-2 text-white hover:bg-blue-700 focus:outline-2 focus:outline-blue-600">
    Guardar
</button>
```

## Reglas esenciales

- **Base:** establece el estilo predeterminado.
    
- **Responsive:** adapta el diseño con `sm:`, `md:` y `lg:`.
    
- **Estado:** responde a interacciones con `hover:`, `focus:` y `disabled:`.
    
- **Composición:** combina varias utilidades en el mismo elemento.
    

**Principio:** define primero el comportamiento base; añade variantes solo cuando necesites modificarlo.