# Tailwind CSS: Tamaño y peso tipográfico

Controla la jerarquía del texto mediante `text-*` para el tamaño y `font-*` para el peso tipográfico.

## Clases esenciales

|Clase|Función|
|---|---|
|`text-sm`|Texto pequeño|
|`text-base`|Texto normal|
|`text-xl`|Texto grande|
|`text-2xl`|Texto extra grande|
|`font-medium`|Peso medio|
|`font-semibold`|Peso seminegrita|
|`font-bold`|Negrita|

## Ejemplo funcional

```html
<article>
    <h2 class="text-xl font-semibold">
        Pedido #1024
    </h2>

    <p class="text-sm">
        Pendiente de revisión.
    </p>
</article>
```

- `text-xl`: destaca el título por su tamaño.
    
- `font-semibold`: aumenta el peso del título.
    
- `text-sm`: reduce el tamaño de la descripción.
    

## Regla esencial

Combina tamaño y peso para diferenciar títulos, contenido y metadatos sin crear reglas CSS personalizadas.

**Principio:** `text-*` controla el tamaño; `font-*` controla el peso tipográfico.