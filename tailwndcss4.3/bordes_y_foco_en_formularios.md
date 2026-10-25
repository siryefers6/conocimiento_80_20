# Tailwind CSS: Bordes y foco en formularios

Define los límites de los campos y destaca el elemento activo. Combina bordes discretos con esquinas redondeadas y un indicador de foco visible.

## 1. Clases esenciales

|Clase|Función|
|---|---|
|`border`|Añade un borde de 1 px|
|`border-gray-200`|Establece un borde gris claro|
|`rounded-md`|Redondea las esquinas|
|`focus:ring-2`|Añade un anillo de foco de 2 px|
|`focus:ring-blue-600`|Establece el color del anillo de foco|
|`focus:outline-none`|Elimina el contorno predeterminado del navegador|

## 2. Ejemplo funcional

```html
<label for="pedido" class="mb-2 block text-sm font-medium">
    Número de pedido
</label>

<input
    id="pedido"
    name="pedido"
    type="text"
    class="w-full rounded-md border border-gray-200 px-3 py-2
           focus:outline-2 focus:outline-blue-600"
/>
```

Este ejemplo usa un contorno visible al recibir el foco, sin depender de clases de anillo.

## 3. Usar anillos en Tailwind CSS v4.3

```html
<input
    type="text"
    class="w-full rounded-md border border-gray-200 px-3 py-2
           focus:border-blue-600 focus:ring-2 focus:ring-blue-600"
/>
```

- `focus:border-blue-600`: cambia el color del borde al enfocar.
    
- `focus:ring-2`: añade un anillo de 2 px.
    
- `focus:ring-blue-600`: define el color del anillo.
    

## Regla esencial

No elimines el indicador de foco sin reemplazarlo por uno visible. Comprueba el contraste y la navegación mediante teclado.

**Principio:** campos delimitados con claridad y foco perceptible.