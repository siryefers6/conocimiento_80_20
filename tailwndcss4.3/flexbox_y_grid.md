# Tailwind CSS: Flexbox y Grid

Distribuye elementos con `flex` o construye estructuras bidimensionales con `grid`. Controla dirección, alineación y separación mediante clases utilitarias.

## Clases esenciales

| Clase             | Función                                      |
| ----------------- | -------------------------------------------- |
| `flex`            | Activa Flexbox                               |
| `flex-col`        | Organiza elementos en columna                |
| `items-center`    | Alinea elementos en el eje transversal       |
| `justify-between` | Distribuye elementos con espacio entre ellos |
| `grid`            | Activa CSS Grid                              |
| `grid-cols-2`     | Crea dos columnas iguales                    |
| `gap-4`           | Establece separación entre elementos         |

## Ejemplo: Flexbox

```html
<div class="flex items-center justify-between gap-4">
    <h2 class="text-lg font-semibold">Pedidos</h2>
    <button class="rounded-lg bg-blue-600 px-4 py-2 text-white">
        Crear
    </button>
</div>
```

- `flex`: coloca los elementos en una fila.
    
- `items-center`: los centra verticalmente.
    
- `justify-between`: coloca uno a cada extremo.
    
- `gap-4`: establece separación entre elementos.
    

## Ejemplo: Grid

```html
<div class="grid grid-cols-2 gap-4">
    <div class="rounded-lg bg-gray-100 p-4">Pedidos</div>
    <div class="rounded-lg bg-gray-100 p-4">Transportistas</div>
</div>
```

Crea dos columnas con separación uniforme.

## Regla esencial

- **Flexbox:** distribuye elementos principalmente en una dimensión: fila o columna.
    
- **Grid:** organiza elementos en filas y columnas.
    

**Principio:** elige Flexbox para alinear componentes; elige Grid para construir estructuras.