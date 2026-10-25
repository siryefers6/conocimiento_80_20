# Tailwind CSS: Detección de clases en Jinja2

Tailwind CSS v4 detecta automáticamente las clases utilizadas en los archivos fuente. Incluye las plantillas Jinja2 dentro de las rutas analizadas.

Ejemplo Jinja2:

```html
<button class="bg-blue-600 text-white rounded-lg p-2">
    Guardar
</button>
```

Tailwind detecta las clases y genera el CSS correspondiente durante la compilación.

Reglas:

- Mantén las plantillas dentro de las rutas detectables.
    
- Usa clases completas y literales.
    
- Evita construir nombres de clases dinámicamente.
    

```html
<!-- Evita: Tailwind no detecta estas clases completas -->
<div class="bg-{{ color }}-600"></div>

<!-- Preferible: clases completas -->
<div class="{{ 'bg-red-600' if error else 'bg-green-600' }}"></div>
```

Si las plantillas están fuera de las rutas detectadas, registra su ubicación en `src/input.css`:

```
@import "tailwindcss";
@source "../templates";
```

Principio: Tailwind genera CSS para las clases que detecta como texto. Asegura que las plantillas Jinja2 sean fuentes reconocidas por el compilador.