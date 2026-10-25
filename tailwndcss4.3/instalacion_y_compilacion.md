# Tailwind CSS: Instalación y compilación

```sh
# Instala Tailwind CSS v4.3 mediante CLI
npm install tailwindcss @tailwindcss/cli
```

## Importar CSS

Crea `src/input.css`:

```
@import "tailwindcss";
```

## Compilar

```sh
# Genera el CSS final
npx @tailwindcss/cli -i ./src/input.css -o ./static/css/output.css
```

## Modo observación

```sh
# Recompila automáticamente al detectar cambios
npx @tailwindcss/cli -i ./src/input.css -o ./static/css/output.css --watch
```

## Conectar con HTML

```html
<link href="/static/css/output.css" rel="stylesheet">
```

Principio: modifica `input.css`; utiliza `output.css` generado. No edites el archivo compilado manualmente. `--watch` automatiza la recompilación durante el desarrollo.