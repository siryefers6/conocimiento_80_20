# Tailwind CSS: Sintaxis de clases utilitarias

Combina clases utilitarias directamente en el HTML para reemplazar reglas CSS habituales. Cada clase aplica una propiedad o un conjunto específico de estilos.

## Espaciado

|Clase|Función|
|---|---|
|`p-4`|Padding en los cuatro lados|
|`m-2`|Margen en los cuatro lados|
|`gap-4`|Espacio entre elementos de un contenedor|

En Tailwind CSS v4, la escala predeterminada usa incrementos de `0.25rem` para estas clases.

## Ejemplo con HTML y Jinja2

```html
<div class="p-4 m-2">
    <div class="flex gap-4">
        <button class="rounded bg-blue-600 px-4 py-2 text-white">
            Guardar
        </button>
        <button class="rounded bg-gray-200 px-4 py-2">
            Cancelar
        </button>
    </div>
</div>
```

- `p-4`: agrega espacio interior.
    
- `m-2`: agrega espacio exterior.
    
- `flex`: organiza los elementos en una fila por defecto.
    
- `gap-4`: separa los elementos.
    
- `px-4` y `py-2`: ajustan el espacio horizontal y vertical.
    
- `rounded`: redondea las esquinas.
    

## Regla esencial

Combina utilidades pequeñas para construir componentes. Evita crear reglas CSS personalizadas cuando las clases existentes resuelvan el mismo problema.

**Principio:** una clase, una función específica; varias clases, un componente completo.