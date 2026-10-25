# Tailwind CSS: Colores de fondo y texto

Controla los colores mediante clases utilitarias. `bg-*` define el fondo; `text-*` define el color del texto.

## Clases esenciales

|Clase|Función|
|---|---|
|`bg-blue-600`|Fondo azul|
|`bg-red-600`|Fondo rojo|
|`text-white`|Texto blanco|
|`text-gray-700`|Texto gris oscuro|

## Ejemplo con HTML y Jinja2

```html
<button class="bg-blue-600 text-white px-4 py-2 rounded">
    Guardar
</button>
```

Combina fondo, texto y espaciado para construir un botón sin escribir CSS personalizado.

## Variantes de color

Ajusta la intensidad con la escala numérica:

```html
<div class="bg-blue-100 text-blue-900">
    Información
</div>

<div class="bg-blue-600 text-white">
    Acción principal
</div>

<div class="bg-blue-900 text-white">
    Acción destacada
</div>
```

## Regla esencial

- `bg-{color}-{intensidad}`: controla el fondo.
    
- `text-{color}-{intensidad}`: controla el texto.
    
- `text-{tamaño}`: también puede controlar el tamaño tipográfico, según la clase utilizada.
    

**Principio:** combina colores con contraste suficiente para mantener la legibilidad.