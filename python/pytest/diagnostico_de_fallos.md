# Diagnóstico de fallos en Pytest

Utiliza las opciones de ejecución para identificar errores, reducir el ruido y localizar problemas de descubrimiento de pruebas.

## Opciones esenciales

| Opción           | Función                                                                               |
| ---------------- | ------------------------------------------------------------------------------------- |
| `-v`             | Muestra el resultado de cada prueba con mayor detalle.                                |
| `-vv`            | Aumenta el detalle de la salida, incluidas representaciones más completas de valores. |
| `-x`             | Detiene la ejecución después del primer fallo.                                        |
| `--tb=short`     | Muestra trazas de error resumidas.                                                    |
| `--collect-only` | Descubre y enumera pruebas sin ejecutarlas.                                           |

## Ejemplos de uso

```sh
# Mostrar cada prueba
uv run pytest -v

# Inspeccionar resultados con más detalle
uv run pytest -vv

# Detenerse en el primer fallo
uv run pytest -x

# Reducir la longitud de las trazas
uv run pytest --tb=short

# Comprobar qué pruebas se detectan
uv run pytest --collect-only

# Combinar opciones
uv run pytest -v -x --tb=short
```

## Flujo de diagnóstico

1. Ejecuta `--collect-only` si sospechas que Pytest no descubre una prueba.
    
2. Usa `-v` para identificar qué pruebas pasan o fallan.
    
3. Añade `-x --tb=short` para localizar rápidamente el primer error.
    
4. Usa `-vv` cuando necesites inspeccionar comparaciones o valores detallados.
    

**Prioriza:** `-v`, `-x` y `--tb=short` en el ciclo diario; reserva `-vv` para casos difíciles y `--collect-only` para problemas de descubrimiento.