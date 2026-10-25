# Tailwind CSS: Bordes y separación visual

Define límites entre componentes mediante `border` y controla el color del borde con `border-*`.

## Clases esenciales

|Clase|Función|
|---|---|
|`border`|Añade un borde de 1 px|
|`border-2`|Añade un borde de 2 px|
|`border-gray-200`|Establece un borde gris claro|
|`border-gray-300`|Establece un borde gris más oscuro|
|`rounded-lg`|Redondea las esquinas|

## Ejemplo funcional

```html
<article class="border border-gray-200 rounded-lg bg-white p-4">
    <h2 class="font-semibold text-gray-900">
        Pedido #1024
    </h2>

    <p class="mt-2 text-sm text-gray-600">
        Pendiente de revisión.
    </p>
</article>
```

- `border`: aplica el borde.
    
- `border-gray-200`: define su color.
    
- `rounded-lg`: suaviza las esquinas.
    
- `p-4`: separa el contenido del borde.
    

## Regla esencial

Combina bordes sutiles con fondos contrastantes para delimitar tarjetas sin recargar la interfaz.

**Principio:** separa visualmente los componentes sin añadir decoración innecesaria.