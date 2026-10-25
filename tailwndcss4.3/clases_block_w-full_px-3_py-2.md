# Tailwind CSS: Formularios y tablas

Construye formularios de gestión con campos legibles y controles que ocupen el espacio disponible. Usa utilidades de visualización, ancho y espaciado interno.

## 1. Clases esenciales

|Clase|Función|
|---|---|
|`block`|Hace que el elemento se comporte como bloque|
|`w-full`|Ocupa el 100 % del ancho disponible|
|`px-3`|Añade espaciado horizontal interno|
|`py-2`|Añade espaciado vertical interno|
|`mb-2`|Añade margen inferior|
|`border`|Añade un borde|

## 2. Ejemplo funcional: formulario

```html
<form class="space-y-4">
    <div>
        <label for="pedido" class="mb-2 block text-sm font-medium">
            Número de pedido
        </label>

        <input
            id="pedido"
            name="pedido"
            type="text"
            class="block w-full rounded-md border border-gray-200 px-3 py-2"
        >
    </div>

    <button
        type="submit"
        class="rounded-md bg-blue-600 px-3 py-2 text-white"
    >
        Buscar
    </button>
</form>
```

- `block`: coloca la etiqueta en su propia línea.
    
- `w-full`: adapta el campo al ancho del contenedor.
    
- `px-3 py-2`: separa el texto de los bordes.
    
- `space-y-4`: mantiene una separación vertical uniforme entre los elementos directos del formulario.
    

## 3. Regla esencial

Usa `block w-full` para campos que deban ocupar toda la fila y `px-* py-*` para mejorar su comodidad de uso.

**Principio:** campos consistentes, etiquetas claras y espaciado uniforme.