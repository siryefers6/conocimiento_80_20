# SQLModel: relaciones muchos-a-muchos con tablas intermedias

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza una tabla intermedia para relacionar múltiples registros de dos entidades. Cada fila conecta una entidad de un lado con otra del lado opuesto mediante claves foráneas.

## 1. Define los modelos

Ejemplo: un producto puede pertenecer a varias categorías y una categoría puede contener varios productos.

```python
from sqlmodel import Field, Relationship, SQLModel


class ProductoCategoria(SQLModel, table=True):
    producto_id: int | None = Field(
        default=None,
        foreign_key="producto.id",
        primary_key=True,
    )
    categoria_id: int | None = Field(
        default=None,
        foreign_key="categoria.id",
        primary_key=True,
    )


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str

    categorias: list["Categoria"] = Relationship(
        back_populates="productos",
        link_model=ProductoCategoria,
    )


class Categoria(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str

    productos: list[Producto] = Relationship(
        back_populates="categorias",
        link_model=ProductoCategoria,
    )
```

## 2. Comprende la tabla intermedia

- `ProductoCategoria`: almacena las asociaciones.
    
- `producto_id`: referencia un producto.
    
- `categoria_id`: referencia una categoría.
    
- La clave primaria compuesta impide duplicar la misma asociación.
    
- `link_model`: indica a SQLModel qué modelo utilizar como tabla de enlace.
    

## 3. Crear una asociación

```python
with Session(engine) as session:
    producto = Producto(nombre="Teclado")
    categoria = Categoria(nombre="Periféricos")

    producto.categorias.append(categoria)

    session.add(producto)
    session.commit()
```

SQLAlchemy gestiona la persistencia del producto, la categoría y su asociación mediante la tabla intermedia.

## Reglas prácticas

- Utiliza `link_model` cuando la tabla intermedia solo almacene las claves de asociación.
    
- Usa una clave primaria compuesta para impedir asociaciones duplicadas.
    
- Si la asociación necesita atributos propios, como fecha, cantidad o estado, modela la tabla intermedia como una entidad explícita.
    
- Define `back_populates` en ambos lados para habilitar la navegación bidireccional.
    

**Prioriza:** tabla intermedia, claves foráneas, `link_model` y cuándo convertir la asociación en una entidad propia.