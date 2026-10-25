# Tailwind CSS: Cuándo elegir Flexbox o Grid

Elige **Flexbox** para distribuir elementos en una dimensión. Elige **Grid** para organizar elementos en filas y columnas.

## Flexbox: una dimensión

Usa `flex` cuando necesites alinear elementos en una fila o columna.

```html
<div class="flex items-center justify-between gap-4">
    <h2>Pedidos</h2>
    <button>Crear</button>
</div>
```

**Casos de uso:**

- Barras de navegación.
    
- Botones alineados.
    
- Encabezados y barras de herramientas.
    
- Formularios organizados verticalmente con `flex-col`.
    

## Grid: dos dimensiones

Usa `grid` cuando necesites controlar simultáneamente filas y columnas.

```html
<div class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3">
    <article>Pedido 1</article>
    <article>Pedido 2</article>
    <article>Pedido 3</article>
</div>
```

**Casos de uso:**

- Cuadrículas de tarjetas.
    
- Paneles de indicadores.
    
- Dashboards.
    
- Distribuciones con varias columnas.
    

## Comparación rápida

|Necesidad|Elige|
|---|---|
|Alinear elementos en una fila|Flexbox|
|Apilar elementos verticalmente|Flexbox|
|Distribuir elementos en columnas iguales|Grid|
|Crear un dashboard de tarjetas|Grid|
|Centrar y distribuir contenido|Flexbox|
|Controlar filas y columnas simultáneamente|Grid|

## Regla esencial

Combina ambos cuando sea necesario. Usa Grid para distribuir las tarjetas del dashboard y Flexbox para alinear el contenido de cada tarjeta.

**Principio:** Flexbox controla la distribución en una dimensión; Grid controla la estructura en dos.