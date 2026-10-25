# Tailwind CSS: Estados e interacciones visuales

Comunica el estado de botones, campos y elementos interactivos mediante variantes. Aplica estilos según la interacción del usuario o el estado del elemento.

## 1. Variantes esenciales

|Variante|Función|
|---|---|
|`hover:`|Cambia el estilo al pasar el puntero|
|`focus:`|Cambia el estilo cuando el elemento recibe el foco|
|`focus-visible:`|Muestra el foco cuando corresponde, especialmente al navegar con teclado|
|`disabled:`|Aplica estilos cuando el elemento está deshabilitado|

## 2. Ejemplo funcional

```html
<button
    class="rounded-md bg-blue-600 px-4 py-2 text-white
           hover:bg-blue-700
           focus-visible:outline-2 focus-visible:outline-offset-2
           focus-visible:outline-blue-600
           disabled:cursor-not-allowed disabled:opacity-50"
>
    Guardar
</button>
```

- `hover:bg-blue-700`: oscurece el botón al pasar el puntero.
    
- `focus-visible:*`: hace visible el foco para facilitar la navegación con teclado.
    
- `disabled:opacity-50`: reduce la opacidad cuando está deshabilitado.
    

## 3. Aplicar estados a un campo

```html
<input
    type="text"
    placeholder="Número de pedido"
    class="rounded-md border border-gray-200 p-2
           focus:border-blue-600 focus:outline-2
           focus:outline-blue-600"
/>
```

`focus:` modifica el aspecto del campo mientras tiene el foco.

## Regla esencial

Las variantes controlan la presentación, no la lógica. Usa atributos HTML como `disabled` para establecer el estado real del elemento.

**Principio:** cada interacción debe comunicar su estado sin depender únicamente del color.