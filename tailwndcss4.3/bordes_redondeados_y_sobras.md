# Tailwind CSS: Bordes redondeados y sombras

Aplica `rounded-*` para redondear esquinas y `shadow-*` para agregar profundidad. Mejora la separación visual de tarjetas, botones y paneles sin escribir CSS personalizado.

## Clases esenciales

|Clase|Función|
|---|---|
|`rounded-lg`|Redondea las esquinas|
|`rounded-full`|Crea esquinas completamente redondeadas|
|`shadow-sm`|Aplica una sombra sutil|
|`shadow-md`|Aplica una sombra media|
|`shadow-lg`|Aplica una sombra más pronunciada|
|`shadow-none`|Elimina la sombra|

## Ejemplo: tarjeta

```html
<div class="rounded-lg bg-white p-4 shadow-md">
    <h2 class="text-lg font-semibold">Pedido #1024</h2>
    <p class="text-gray-600">Pendiente de revisión.</p>
</div>
```

- `rounded-lg`: suaviza las esquinas.
    
- `bg-white`: establece el fondo.
    
- `p-4`: agrega espacio interior.
    
- `shadow-md`: separa visualmente la tarjeta del fondo.
    

## Regla esencial

Usa sombras moderadas y redondeos consistentes. Reserva las sombras más pronunciadas para elementos que necesiten destacar.

**Principio:** `rounded-*` define la forma; `shadow-*` define la profundidad.