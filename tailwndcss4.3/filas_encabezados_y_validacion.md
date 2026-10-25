# Tailwind CSS: Filas, encabezados y validación

Construye interfaces de gestión legibles mediante encabezados diferenciados, filas consistentes, botones reconocibles y mensajes de validación visibles.

## 1. Encabezados y filas de tablas

```html
<table class="w-full text-sm">
    <thead class="bg-gray-100 text-gray-600">
        <tr>
            <th class="px-3 py-2 text-left font-semibold">Pedido</th>
            <th class="px-3 py-2 text-left font-semibold">Estado</th>
        </tr>
    </thead>

    <tbody>
        <tr class="border-b border-gray-200 hover:bg-gray-100">
            <td class="px-3 py-2">1024</td>
            <td class="px-3 py-2">Pendiente</td>
        </tr>
    </tbody>
</table>
```

- `bg-gray-100`: diferencia visualmente el encabezado.
    
- `text-left`: alinea el contenido a la izquierda.
    
- `border-b`: separa las filas.
    
- `hover:bg-gray-100`: destaca la fila bajo el puntero.
    

## 2. Botones

```html
<button
    type="submit"
    class="rounded-md bg-blue-600 px-3 py-2 text-white
           hover:bg-blue-700 transition-colors"
>
    Guardar
</button>
```

- `bg-blue-600`: identifica la acción principal.
    
- `hover:bg-blue-700`: comunica la interacción.
    
- `transition-colors`: suaviza el cambio de color.
    

## 3. Mensajes de validación

```html
<label for="pedido" class="mb-2 block text-sm font-medium">
    Número de pedido
</label>

<input
    id="pedido"
    name="pedido"
    aria-invalid="true"
    aria-describedby="error-pedido"
    class="w-full rounded-md border border-red-600 px-3 py-2"
/>

<p id="error-pedido" class="mt-1 text-sm text-red-700">
    Introduce un número de pedido válido.
</p>
```

- `border-red-600`: identifica visualmente el campo con error.
    
- `text-red-700`: destaca el mensaje.
    
- `aria-invalid="true"`: comunica el estado de error a tecnologías de asistencia.
    
- `aria-describedby`: relaciona el campo con su mensaje de validación.
    

## Regla esencial

Mantén los estilos consistentes. No comuniques errores únicamente mediante color: añade un mensaje textual y vincúlalo al campo correspondiente.

**Principio:** estructura clara, acciones reconocibles y errores comprensibles.