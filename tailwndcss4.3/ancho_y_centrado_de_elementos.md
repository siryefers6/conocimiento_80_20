# Tailwind CSS: Ancho y centrado de elementos

Controla el ancho y la posición horizontal mediante utilidades. Combina `w-full`, `max-w-*` y `mx-auto` para crear formularios y contenedores adaptables.

## Clases esenciales

| Clase      | Función                                   |
| ---------- | ----------------------------------------- |
| `w-full`   | Ocupa el 100 % del ancho disponible       |
| `max-w-xl` | Limita el ancho máximo a `36rem` (576 px) |
| `mx-auto`  | Aplica márgenes horizontales automáticos  |

## Ejemplo con Jinja2

```html
<div class="mx-auto w-full max-w-xl p-4">
    <form class="space-y-4">
        <input
            type="text"
            name="nombre"
            placeholder="Nombre"
            class="w-full rounded-lg border p-2"
        >

        <button
            type="submit"
            class="w-full rounded-lg bg-blue-600 p-2 text-white"
        >
            Registrar
        </button>
    </form>
</div>
```

## Cómo funciona

1. `w-full` permite ocupar todo el ancho disponible.
    
2. `max-w-xl` evita que el contenedor crezca demasiado.
    
3. `mx-auto` centra el contenedor cuando existe espacio horizontal sobrante.
    

## Regla esencial

Combina `w-full max-w-xl mx-auto` para obtener un contenedor adaptable y centrado.

**Principio:** ancho flexible, límite máximo y márgenes automáticos.