# Tailwind CSS: Crear cuadrículas con Grid

Usa `grid` para activar CSS Grid, `grid-cols-2` para definir dos columnas y `gap-4` para separar los elementos.

## Clases esenciales

|Clase|Función|
|---|---|
|`grid`|Activa CSS Grid|
|`grid-cols-2`|Define dos columnas iguales|
|`grid-cols-3`|Define tres columnas iguales|
|`gap-4`|Separa filas y columnas|
|`col-span-2`|Ocupa dos columnas|

## Ejemplo funcional

```html
<div class="grid grid-cols-2 gap-4">
    <div class="rounded-lg bg-gray-100 p-4">Pedidos</div>
    <div class="rounded-lg bg-gray-100 p-4">Transportistas</div>
    <div class="col-span-2 rounded-lg bg-blue-100 p-4">
        Resumen general
    </div>
</div>
```

- `grid-cols-2`: distribuye los elementos en dos columnas.
    
- `gap-4`: añade separación entre filas y columnas.
    
- `col-span-2`: hace que el resumen ocupe ambas columnas.
    

## Diseño adaptable

Combina variantes responsive para ajustar las columnas según el ancho de pantalla:

```html
<div class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3">
    <!-- Tarjetas -->
</div>
```

- Móvil: una columna.
    
- `md`: dos columnas.
    
- `lg`: tres columnas.
    

## Regla esencial

Usa Grid para organizar tarjetas, indicadores y paneles en dos dimensiones.

**Principio:** define las columnas, establece la separación y adapta la cuadrícula al dispositivo.