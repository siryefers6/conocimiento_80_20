# Tailwind CSS: Ancho máximo y distribución por pantalla

Combina `max-w-*`, `mx-auto` y variantes responsive para limitar el ancho del contenido y distribuir elementos según el espacio disponible.

## Clases esenciales

| Clase            | Función                            |
| ---------------- | ---------------------------------- |
| `w-full`         | Ocupa todo el ancho disponible     |
| `max-w-xl`       | Limita el ancho máximo a 36 rem    |
| `mx-auto`        | Centra horizontalmente el elemento |
| `grid-cols-1`    | Define una columna                 |
| `md:grid-cols-2` | Define dos columnas desde `md`     |
| `lg:grid-cols-3` | Define tres columnas desde `lg`    |

## Ejemplo: contenedor adaptable

```html
<main class="mx-auto w-full max-w-6xl p-4">
    <div class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3">
        <article class="rounded-lg bg-white p-4 shadow-md">
            Pedidos
        </article>
        <article class="rounded-lg bg-white p-4 shadow-md">
            Transportistas
        </article>
        <article class="rounded-lg bg-white p-4 shadow-md">
            Estadísticas
        </article>
    </div>
</main>
```

## Cómo funciona

1. `w-full`: permite que el contenedor ocupe el ancho disponible.
    
2. `max-w-6xl`: limita su crecimiento a 72 rem.
    
3. `mx-auto`: centra el contenedor cuando sobra espacio.
    
4. `grid-cols-1`: muestra una columna en móviles.
    
5. `md:grid-cols-2`: distribuye dos columnas desde 768 px.
    
6. `lg:grid-cols-3`: distribuye tres columnas desde 1024 px.
    

## Regla esencial

Limita el ancho del contenedor y adapta la distribución interior. No confundas el ancho máximo de la página con el número de columnas.

**Principio:** contenedor limitado, centrado y distribución responsive.