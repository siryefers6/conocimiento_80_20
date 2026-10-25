# SQLModel 0.0.48: migraciones de esquema

**Referencia:** SQLModel 0.0.48 · SQLAlchemy 2.x · 24-10-2026

## 1. Comprende las migraciones

Una migración describe cómo cambiar el esquema de una base de datos existente sin perder los datos.

Ejemplos:

- Añadir, renombrar o eliminar columnas.
    
- Crear índices y restricciones.
    
- Modificar tipos de datos.
    
- Añadir tablas y relaciones.
    

`SQLModel.metadata.create_all(engine)` crea tablas inexistentes, pero **no modifica las tablas existentes** para reflejar los cambios en los modelos.

## 2. Herramienta y flujo

Alembic es la herramienta habitual para gestionar migraciones con SQLAlchemy. No forma parte del núcleo de SQLModel.

Flujo básico:

1. Modifica el modelo SQLModel.
    
2. Genera una migración candidata.
    
3. Revisa las operaciones generadas.
    
4. Aplica la migración a la base de datos.
    
5. Ejecuta pruebas de integración.
    

```sh
uv add alembic
alembic init migrations
alembic revision --autogenerate -m "agregar columna estado"
alembic upgrade head
```

Configura `migrations/env.py` para importar los modelos y asignar `SQLModel.metadata` a `target_metadata`. Configura también la URL de conexión.

## 3. Migraciones automáticas y manuales

- **Autogeneración:** compara los metadatos de los modelos con el esquema detectado.
    
- **Revisión manual:** corrige o completa las operaciones propuestas.
    
- **Migración manual:** necesaria cuando el cambio requiere transformar datos o ejecutar operaciones que no se detectan automáticamente.
    

No apliques una migración autogenerada sin revisarla.

## 4. Evoluciona esquemas con seguridad

- Haz copias de seguridad antes de modificar datos importantes.
    
- Añade columnas obligatorias mediante una estrategia compatible con los registros existentes: valor por defecto, columna temporal o actualización de datos.
    
- Trata los cambios de nombre como renombrados explícitos cuando corresponda; no asumas que Alembic los identificará correctamente.
    
- Divide los cambios incompatibles en varias migraciones.
    
- Prueba tanto la actualización como la recuperación cuando exista una operación reversible.
    

## 5. SQLite y PostgreSQL

- **SQLite:** algunas alteraciones requieren reconstruir tablas; Alembic puede utilizar el modo _batch_.
    
- **PostgreSQL:** admite más operaciones directas, aunque ciertas modificaciones pueden bloquear tablas o requerir planificación.
    
- Ejecuta las migraciones en una copia representativa de producción antes del despliegue.
    

## 6. Comandos esenciales

```sh
# Crear una migración
alembic revision --autogenerate -m "descripcion"

# Aplicar todas las migraciones pendientes
alembic upgrade head

# Consultar la revisión actual
alembic current

# Consultar el historial
alembic history

# Retroceder una revisión
alembic downgrade -1
```

**Orden recomendado:** concepto de migración → configuración de Alembic → revisión de autogeneraciones → transformaciones de datos → estrategia de despliegue y recuperación.

**Prioriza:** aprende a revisar y probar cada migración. El archivo de migración versionado, no `create_all()`, debe ser la fuente de cambios del esquema en producción.

**Referencias:**

- [Alembic: tutorial](https://alembic.sqlalchemy.org/en/latest/tutorial.html)
    
- [Alembic: autogeneración de migraciones](https://alembic.sqlalchemy.org/en/latest/autogenerate.html)
    
- [Alembic: migraciones por lotes para SQLite](https://alembic.sqlalchemy.org/en/latest/batch.html)