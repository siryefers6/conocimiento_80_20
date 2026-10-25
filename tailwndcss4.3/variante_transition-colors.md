# Tailwind CSS: `transition-colors`

Suaviza los cambios de color cuando un elemento cambia de estado. Combínalo con variantes como `hover:` y `focus:` para mejorar la respuesta visual.

## Ejemplo funcional

```html
<button
    class="rounded-md bg-blue-600 px-4 py-2 text-white
           transition-colors duration-200
           hover:bg-blue-700"
>
    Guardar
</button>
```

- `transition-colors`: anima los cambios de color.
    
- `duration-200`: establece una duración de 200 milisegundos.
    
- `hover:bg-blue-700`: cambia el fondo al pasar el puntero.
    

## Variantes relacionadas

|Clase|Función|
|---|---|
|`transition-colors`|Transiciona colores de texto, fondo, borde y otros elementos compatibles|
|`transition-opacity`|Transiciona la opacidad|
|`transition-transform`|Transiciona transformaciones|
|`duration-200`|Define la duración de la transición|

## Regla esencial

Usa transiciones breves para cambios de estado. Evita animar propiedades innecesarias o añadir efectos que retrasen la interacción.

**Principio:** transiciones sutiles, respuesta inmediata.