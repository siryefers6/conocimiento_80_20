# Tailwind CSS: `overflow-x-auto`

Permite desplazar horizontalmente un contenedor cuando su contenido supera el ancho disponible. Úsalo para tablas con muchas columnas en pantallas pequeñas.

## Ejemplo funcional

```html
<div class="overflow-x-auto">
    <table class="w-full border-collapse text-sm">
        <thead>
            <tr class="border-b">
                <th class="p-3 text-left">Pedido</th>
                <th class="p-3 text-left">Transportista</th>
                <th class="p-3 text-left">Estado</th>
                <th class="p-3 text-left">Fecha de registro</th>
            </tr>
        </thead>
        <tbody>
            <tr class="border-b">
                <td class="p-3">1024</td>
                <td class="p-3">Juan Pérez</td>
                <td class="p-3">Pendiente</td>
                <td class="p-3">24-10-2026</td>
            </tr>
        </tbody>
    </table>
</div>
```

- `overflow-x-auto`: habilita el desplazamiento horizontal cuando es necesario.
    
- `w-full`: permite que la tabla ocupe el ancho disponible.
    
- `border-collapse`: combina los bordes de las celdas.
    

## Clases relacionadas

|Clase|Función|
|---|---|
|`overflow-x-auto`|Muestra desplazamiento horizontal cuando hace falta|
|`overflow-x-hidden`|Oculta el desbordamiento horizontal|
|`overflow-x-scroll`|Mantiene disponible el desplazamiento horizontal|
|`overflow-y-auto`|Permite desplazamiento vertical cuando hace falta|

## Regla esencial

Aplica `overflow-x-auto` al contenedor de la tabla, no directamente a cada celda. Así mantienes la estructura tabular y permites consultar todas las columnas en pantallas estrechas.

**Principio:** conserva los datos; adapta el desplazamiento al espacio disponible.