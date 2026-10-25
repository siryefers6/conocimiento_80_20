# Tailwind CSS: Variante `focus-visible:`

Muestra un indicador visual de foco cuando el navegador determina que es necesario, especialmente durante la navegación con teclado. Mejora la accesibilidad sin mostrar el indicador en cada interacción con el ratón.

## 1. Ejemplo funcional

```html
<button
    class="rounded-md bg-blue-600 px-4 py-2 text-white
           focus-visible:outline-2
           focus-visible:outline-offset-2
           focus-visible:outline-blue-600"
>
    Guardar
</button>
```

- `focus-visible:outline-2`: define el grosor del contorno.
    
- `focus-visible:outline-offset-2`: separa el contorno del botón.
    
- `focus-visible:outline-blue-600`: establece su color.
    

## 2. Diferencia entre `focus:` y `focus-visible:`

|Variante|Comportamiento|
|---|---|
|`focus:`|Aplica estilos siempre que el elemento tenga el foco|
|`focus-visible:`|Aplica estilos cuando el navegador considera necesario mostrar el indicador de foco|

## Regla esencial

No elimines el indicador de foco sin proporcionar una alternativa visible. Permite navegar y operar los controles mediante teclado.

**Principio:** foco visible, navegación accesible.