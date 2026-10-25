# SQLModel: claves primarias y restricciones

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Define restricciones para identificar registros y garantizar la integridad de los datos almacenados en la base de datos.

## 1. Clave primaria: `primary_key`

Identifica cada registro de forma única.

```python
id: int | None = Field(default=None, primary_key=True)
```

- Impide claves primarias duplicadas.
    
- Identifica registros para consultas, actualizaciones y eliminaciones.
    
- Permite que la base de datos genere el identificador cuando corresponda.
    

## 2. Valores únicos: `unique`

Impide duplicados en una columna.

```python
codigo: str = Field(unique=True)
```

Utilízalo para códigos de barras, correos electrónicos o identificadores comerciales que deban ser únicos.

## 3. Valores nulos: `nullable`

Controla si una columna admite `NULL` en la base de datos.

```python
nombre: str = Field(nullable=False)
descripcion: str | None = Field(default=None, nullable=True)
```

- `nullable=False`: exige un valor no nulo en SQL.
    
- `nullable=True`: permite `NULL` en SQL.
    
- `str | None`: permite `None` en la validación de Python, pero no sustituye por sí solo la configuración SQL explícita.
    

## 4. Integridad referencial

Utiliza claves foráneas para impedir referencias a registros inexistentes.

```python
from sqlmodel import Field, SQLModel


class Pedido(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)


class Bulto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    pedido_id: int = Field(foreign_key="pedido.id")
```

La clave foránea vincula cada bulto con un pedido existente. Configura las reglas de eliminación según las necesidades del dominio.

## Reglas prácticas

- Define una clave primaria para cada tabla.
    
- Usa `unique=True` cuando el dominio prohíba duplicados.
    
- Distingue campos obligatorios de campos opcionales.
    
- Utiliza claves foráneas para mantener relaciones válidas.
    
- Verifica que las restricciones se apliquen realmente en la base de datos.
    

**Prioriza:** `primary_key`, `unique`, `nullable` y `foreign_key`. Comprende que la validación de Python no reemplaza las restricciones de integridad SQL.