Objetivo: distinguir entre mostrar datos de forma segura y comprobar que sean válidos.

- Escape HTML: evita que caracteres especiales se interpreten como código HTML.
    
- Validación: comprueba que los datos cumplan las reglas esperadas: tipo, formato, longitud o rango.
    

## Ejemplo

```
from pydantic import BaseModel, Fieldclass Usuario(BaseModel):    nombre: str = Field(min_length=2, max_length=50)
```

```
<p>{{ usuario.nombre }}</p>
```

Pydantic valida el nombre. Jinja2 escapa su contenido al mostrarlo, si el autoescaping está activado.

Regla: valida al recibir datos; escapa al mostrarlos. Son mecanismos complementarios.