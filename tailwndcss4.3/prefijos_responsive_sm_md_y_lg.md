# Tailwind CSS: Prefijos responsive `sm:`, `md:` y `lg:`

Usa prefijos responsive para aplicar estilos desde un ancho mínimo de pantalla. Tailwind CSS sigue un enfoque **mobile-first**: las clases sin prefijo definen el estilo inicial y las variantes lo modifican en pantallas mayores.

## Prefijos esenciales

| Prefijo | Ancho mínimo |
| ------- | ------------ |
| `sm:`   | 640 px       |
| `md:`   | 768 px       |
| `lg:`   | 1024 px      |
| `xl:`   | 1280 px      |
| `2xl:`  | 1536 px      |

## Ejemplo: columnas adaptables

```html
<div class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3">
    <article>Pedido 1</article>
    <article>Pedido 2</article>
    <article>Pedido 3</article>
</div>
```

- Móvil: una columna.
    
- Desde `md` (768 px): dos columnas.
    
- Desde `lg` (1024 px): tres columnas.
    

## Ejemplo: cambiar la dirección

```html
<div class="flex flex-col gap-2 sm:flex-row">
    <button>Guardar</button>
    <button>Cancelar</button>
</div>
```

Los botones aparecen en columna en móviles y en fila desde los 640 px.

## Reglas esenciales

- Las clases sin prefijo se aplican en todos los tamaños, salvo que otra regla las sobrescriba.
    
- Los prefijos indican un **ancho mínimo**, no un rango exclusivo.
    
- Las variantes mayores mantienen sus efectos en anchos superiores, salvo que otra regla los modifique.
    
- No agregues prefijos innecesarios si el diseño no necesita cambiar.
    

**Principio:** define el diseño móvil primero; añade `sm:`, `md:` y `lg:` cuando el espacio disponible exija otra distribución.