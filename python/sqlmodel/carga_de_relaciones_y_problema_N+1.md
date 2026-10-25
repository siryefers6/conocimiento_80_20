# SQLModel: carga de relaciones y problema N+1

**Versión:** SQLModel 0.0.48 · SQLAlchemy 2.x · **Referencia:** 24-10-2026

## Concepto

El problema **N+1** ocurre cuando una consulta obtiene una lista de registros y luego ejecuta consultas adicionales para cargar las relaciones de cada registro.

Si consultas 100 pedidos y accedes a los bultos de cada uno, puedes terminar ejecutando 101 consultas en lugar de cargar los datos relacionados de forma eficiente.

## 1. Carga diferida (_lazy loading_)

Por defecto, las relaciones de SQLAlchemy suelen cargarse cuando accedes a ellas.

```python
pedidos = session.exec(select(Pedido)).all()

for pedido in pedidos:
    print(pedido.bultos)  # Puede ejecutar una consulta por pedido
```

Evita acceder repetidamente a relaciones sin analizar cómo se cargan.

## 2. `selectinload()`: carga en consultas adicionales agrupadas

Recupera los registros principales y después carga las relaciones mediante una consulta adicional que utiliza los identificadores de los registros obtenidos.

```python
from sqlalchemy.orm import selectinload
from sqlmodel import select


consulta = select(Pedido).options(
    selectinload(Pedido.bultos)
)

pedidos = session.exec(consulta).all()

for pedido in pedidos:
    print(pedido.bultos)
```

**Prioriza** `**selectinload()**` para relaciones de tipo colección, como un pedido con muchos bultos. Reduce el problema N+1 sin multiplicar las filas principales mediante un `JOIN`.

## 3. `joinedload()`: carga mediante JOIN

Carga la relación en la misma consulta SQL mediante una unión.

```python
from sqlalchemy.orm import joinedload
from sqlmodel import select


consulta = select(Pedido).options(
    joinedload(Pedido.cliente)
)

pedidos = session.exec(consulta).all()
```

Es especialmente útil para relaciones de muchos-a-uno o uno-a-uno, como un pedido y su cliente.

Para colecciones, `joinedload()` puede multiplicar las filas SQL. Si seleccionas entidades con una colección cargada de esta forma, utiliza `unique()` cuando sea necesario:

```python
pedidos = (
    session.exec(
        select(Pedido).options(joinedload(Pedido.bultos))
    )
    .unique()
    .all()
)
```

## Comparación rápida

| Estrategia       | Consultas                                | Uso habitual                          |
| ---------------- | ---------------------------------------- | ------------------------------------- |
| Carga diferida   | Puede producir N+1                       | Relaciones que rara vez se utilizan   |
| `selectinload()` | Consulta principal y consultas agrupadas | Colecciones y relaciones uno-a-muchos |
| `joinedload()`   | Una consulta SQL con JOIN                | Relaciones muchos-a-uno o uno-a-uno   |

## Reglas prácticas

- Usa `selectinload()` como primera opción para cargar colecciones relacionadas.
    
- Usa `joinedload()` para relaciones individuales cuando convenga reducir consultas.
    
- Combina estrategias cuando necesites cargar varias relaciones.
    
- No confundas `join()` con `joinedload()`: `join()` modifica la consulta y sus resultados; `joinedload()` optimiza la carga de relaciones sin cambiar la selección lógica de entidades.
    
- Comprueba las consultas SQL ejecutadas y mide el rendimiento con datos representativos.
    
- Si serializas modelos después de cerrar la sesión, carga previamente las relaciones necesarias para evitar errores de acceso a datos no cargados.
    

## Prioriza

1. Reconoce el patrón N+1.
    
2. Aprende `selectinload()` para colecciones.
    
3. Aprende `joinedload()` para relaciones individuales.
    
4. Verifica el número de consultas y el tiempo de respuesta.