# Pydantic: campos obligatorios, opcionales y valores predeterminados

Define la obligatoriedad y los valores iniciales con anotaciones de tipos y `Field`.

## Campos obligatorios y opcionales

```python
from pydantic import BaseModel

class Usuario(BaseModel):
    nombre: str
    edad: int | None
    correo: str | None = None
```

- `nombre: str`: obligatorio y no nulo.
    
- `edad: int | None`: obligatorio; acepta un entero o `None`.
    
- `correo: str | None = None`: opcional; si se omite, usa `None`.
    

**Importante:** `T | None` permite `None`, pero no hace que el campo sea opcional.

## Valores predeterminados

```python
from pydantic import BaseModel, Field

class Pedido(BaseModel):
    cantidad: int = 1
    etiquetas: list[str] = Field(default_factory=list)
```

- `default`: establece un valor predeterminado.
    
- `default_factory`: genera un valor nuevo para cada instancia.
    
- `Field`: configura valores predeterminados, restricciones y metadatos.
    

Evita usar listas o diccionarios mutables como valores predeterminados explícitos. Prefiere `default_factory`.

## Campo ausente frente a `None`

```python
usuario = Usuario(nombre="Ana", edad=25)

print(usuario.correo)  # None
print(usuario.model_fields_set)  # {'nombre', 'edad'}
```

`model_fields_set` identifica los campos proporcionados explícitamente al crear el modelo.

```python
usuario = Usuario(nombre="Ana", edad=25, correo=None)

print(usuario.model_fields_set)
# {'nombre', 'edad', 'correo'}
```

La diferencia importa en actualizaciones parciales de API:

```python
datos = usuario.model_dump(exclude_unset=True)
```

`exclude_unset=True` omite los campos no proporcionados explícitamente, aunque tengan valores predeterminados.

**Regla:** distingue entre obligatoriedad, nulabilidad y valor predeterminado. No uses `T | None` como sustituto de un valor predeterminado.