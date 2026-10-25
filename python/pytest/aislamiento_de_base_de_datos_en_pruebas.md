# Aislamiento de base de datos en pruebas

Aísla cada prueba para evitar que los datos creados, modificados o eliminados afecten a otras pruebas o a la base de datos de desarrollo.

## Estrategias principales

- **Base de datos de prueba:** utiliza una base separada de la de desarrollo y producción.
    
- **Transacciones:** ejecuta cada prueba dentro de una transacción y reviértela al finalizar.
    
- **Limpieza:** elimina los datos temporales cuando no puedas utilizar transacciones.
    
- **Fixtures:** centraliza la creación y destrucción de conexiones y sesiones.
    

## Ejemplo con SQLite y SQLModel

```python
import pytest
from sqlmodel import SQLModel, Session, create_engine

@pytest.fixture
def session():
    engine = create_engine("sqlite://")
    SQLModel.metadata.create_all(engine)

    with Session(engine) as session:
        yield session

    SQLModel.metadata.drop_all(engine)
    engine.dispose()
```

Cada prueba recibe una sesión conectada a una base SQLite temporal en memoria.

## Reglas

- Nunca ejecutes pruebas sobre la base de datos real.
    
- No dependas del orden de ejecución de las pruebas.
    
- Usa datos independientes por prueba.
    
- Para probar servicios con FastAPI, inyecta la sesión de prueba mediante `app.dependency_overrides`.
    
- Para pruebas que comparten una conexión, considera transacciones con `rollback()` para deshacer los cambios.
    

**Prioriza:** base de prueba independiente, fixtures y rollback. El aislamiento debe garantizar que cada prueba pueda ejecutarse sola y repetirse sin dejar datos residuales.