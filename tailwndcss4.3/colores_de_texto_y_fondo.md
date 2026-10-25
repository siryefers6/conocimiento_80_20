# Tailwind CSS: Colores de texto y fondo

Define jerarquías visuales mediante colores de texto y superficies. Mantén contraste suficiente para facilitar la lectura.

## 1. Clases esenciales

|Clase|Función|
|---|---|
|`text-gray-600`|Texto secundario gris|
|`text-gray-900`|Texto principal oscuro|
|`text-white`|Texto blanco|
|`bg-white`|Fondo blanco|
|`bg-gray-100`|Fondo gris claro|
|`bg-blue-600`|Fondo azul intenso|

## 2. Ejemplo funcional

```html
<article class="bg-white p-4">
    <h2 class="text-gray-900 text-xl font-semibold">
        Pedido #1024
    </h2>

    <p class="text-gray-600 text-sm">
        Pendiente de revisión.
    </p>
</article>
```

- `text-gray-900`: destaca el contenido principal.
    
- `text-gray-600`: diferencia la información secundaria.
    
- `bg-white`: establece una superficie blanca.
    

## Regla esencial

Usa colores oscuros para el contenido principal y tonos más suaves para información secundaria. Comprueba siempre el contraste.

**Principio:** jerarquía visual mediante contraste, no mediante colores arbitrarios.