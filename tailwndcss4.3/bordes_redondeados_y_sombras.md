# Tailwind CSS: Bordes redondeados y sombras

Define la forma y profundidad de los componentes mediante `rounded-*` y `shadow-*`. Prioriza efectos sutiles para mantener una interfaz limpia.

## 1. Bordes redondeados

|Clase|Función|
|---|---|
|`rounded-sm`|Redondeo pequeño|
|`rounded-md`|Redondeo mediano|
|`rounded-lg`|Redondeo grande|
|`rounded-full`|Forma completamente redondeada|

## 2. Sombras

|Clase|Función|
|---|---|
|`shadow-xs`|Sombra mínima|
|`shadow-sm`|Sombra pequeña|
|`shadow-md`|Sombra mediana|
|`shadow-lg`|Sombra grande|
|`shadow-none`|Elimina la sombra|

## 3. Ejemplo funcional

```html
<article class="rounded-md border border-gray-200 bg-white p-4 shadow-sm">
    <h2 class="font-semibold text-gray-900">
        Pedido #1024
    </h2>

    <p class="mt-2 text-sm text-gray-600">
        Pendiente de revisión.
    </p>
</article>
```

- `rounded-md`: suaviza las esquinas.
    
- `shadow-sm`: añade profundidad discreta.
    
- `border border-gray-200`: delimita la tarjeta.
    

## Regla esencial

Usa redondeos y sombras consistentes. No añadas profundidad a todos los elementos si los bordes ya permiten distinguirlos.

**Principio:** forma y profundidad al servicio de la estructura visual.