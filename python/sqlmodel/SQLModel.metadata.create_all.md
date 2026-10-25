# SQLModel: `SQLModel.metadata.create_all()`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `SQLModel.metadata.create_all(engine)` para crear las tablas que aún no existen en la base de datos, a partir de los modelos de tabla registrados.

## Ejemplo funcional

```python
from sqlmodel import Field, SQLModel, create_engine


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str


engine = create_engine("sqlite:///tienda.db")

SQLModel.metadata.create_all(engine)
```

## Reglas esenciales

- **Importa los modelos antes de ejecutar** `**create_all()**`**.** SQLModel debe conocer sus tablas.
    
- **Crea solo las tablas inexistentes.** No actualiza automáticamente las estructuras existentes.
    
- **Ejecuta la operación después de configurar** `**engine**`**.**
    
- **No lo uses como sistema de migraciones.** Para modificar tablas existentes, aplica migraciones SQL controladas o una herramienta de migraciones.
    

## Verificar las tablas registradas

```python
print(SQLModel.metadata.tables.keys())
```

Comprueba que los modelos esperados estén registrados antes de crear las tablas.

**Prioriza:** registro de modelos, orden de inicialización y diferencia entre creación inicial y migraciones de esquema.