# Convenciones de base de datos

> Reglas y estándares de modelado de datos en Gestor de Tareas.
> Para el modelo de datos concreto del proyecto ver
> [`../architecture/database.md`](../architecture/database.md).
> **Última actualización**: 2026-07-02

## Stack

- **Motor**: PostgreSQL (driver `psycopg2-binary`).
- **Capa de acceso / ORM**: el ORM de Django (modelos en `models.py` de cada app).
- **Migraciones**: sistema de migraciones de Django (`makemigrations` / `migrate`).

## Reglas de modelado

- **Primary keys**: autoincremental (`id` `BigAutoField`, el valor por defecto de
  Django) de forma consistente, salvo justificación explícita.
- **Nombres**: los modelos se escriben en `CamelCase` y sus campos en `snake_case`,
  en inglés (p. ej. `Task`, `title`, `created_at`); Django deriva los nombres de
  tabla en `snake_case` (`tareas_task`). Los textos visibles al usuario van en
  español (ver [`i18n.md`](i18n.md)).
- **Timestamps**: se registra la fecha de creación con `created_at`
  (`DateTimeField(auto_now_add=True)`). Se **recomienda** añadir `updated_at`
  (`DateTimeField(auto_now=True)`) a los modelos que cambian tras su creación;
  el modelo `Task` actual solo tiene `created_at`.
- **Foreign keys**: se declaran con `models.ForeignKey`; Django crea el índice
  automáticamente. Definir siempre `on_delete` (p. ej. `CASCADE`) y `related_name`.
  `NOT NULL` (sin `null=True`) salvo justificación explícita.
- **Tipos preferidos** (campos de Django):

| Caso              | Tipo               |
| ----------------- | ------------------ |
| Email             | `EmailField`       |
| Texto corto       | `CharField(max_length=...)` |
| Texto largo       | `TextField`        |
| JSON estructurado | `JSONField`        |
| Dinero            | `DecimalField(max_digits, decimal_places)` |
| Booleano          | `BooleanField(default=...)` |

## Migraciones

- Reversibles y no destructivas siempre que sea posible.
- Una migración por cambio lógico; nunca editar una migración ya aplicada en `main`.
- Revisar el impacto en datos existentes antes de aplicar en producción.

## Ejemplos

```python
# tareas/models.py
class Task(models.Model):
    title = models.CharField(max_length=255)                  # texto corto, not null
    description = models.TextField(blank=True, null=True)      # texto largo, opcional
    user = models.ForeignKey(                                  # fk indexada, not null
        User, on_delete=models.CASCADE, related_name="tasks"
    )
    category = models.CharField(max_length=100)
    created_at = models.DateTimeField(auto_now_add=True)       # timestamp de creación

    class Meta:
        ordering = ["-created_at"]  # más recientes primero
```

## Comandos útiles

```bash
python manage.py makemigrations            # crear migraciones a partir de los modelos
python manage.py migrate                   # aplicar migraciones pendientes
python manage.py migrate <app> <numero>    # revertir a una migración anterior (rollback)
python manage.py showmigrations            # ver estado de las migraciones
```

## Referencias

- [Modelos de Django](https://docs.djangoproject.com/en/5.1/topics/db/models/)
- [Migraciones de Django](https://docs.djangoproject.com/en/5.1/topics/migrations/)
- [`../architecture/database.md`](../architecture/database.md)
