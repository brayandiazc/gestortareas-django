# Referencia de API

> Endpoints, autenticación y convenciones de la API de **Gestor de Tareas**.
> Documentación interactiva: No aplica — esta versión no expone una API.
>
> **Última actualización**: 2026-07-02

> **Nota importante**: En su versión actual, **Gestor de Tareas no expone una API REST**.
> Es una aplicación web Django monolítica renderizada del lado del servidor (server-side
> rendering con plantillas de Django): la interacción se hace por HTML/formularios, no por
> endpoints JSON. Una API REST está contemplada en el [roadmap](../product/roadmap.md), pero
> aún no existe. El resto de este documento describe las **convenciones previstas** para
> cuando se implemente; no documenta endpoints reales actuales.

## Convenciones generales

- **URL base**: No aplica actualmente. Las rutas web actuales (server-rendered) son `/`, `/tareas/`, `/registro/` y `/accounts/`, no endpoints de API.
- **Versionado**: prefijo de versión en la ruta (p. ej. `/v1`) — previsto para la futura API.
- **Formato**: JSON (`Content-Type: application/json`) — previsto para la futura API.
- **Fechas**: ISO 8601 (`YYYY-MM-DDTHH:mm:ssZ`).

## Autenticación de la API

- No aplica en esta versión: la autenticación actual es por **sesión de Django** (cookies), no por token de API.
- Esquema previsto para una futura API: por definir (p. ej. token de Django REST Framework).
- Ver [`auth.md`](auth.md) para el detalle de la autenticación actual por sesión.

## Manejo de errores

No aplica actualmente (no hay API). Formato de error estándar previsto para la futura API:

```json
{
  "error": {
    "code": "validation_error",
    "message": "El título es obligatorio",
    "details": []
  }
}
```

| Código HTTP | Significado             |
| ----------- | ----------------------- |
| 200 / 201   | Éxito                   |
| 400         | Solicitud inválida      |
| 401         | No autenticado          |
| 403         | No autorizado           |
| 404         | No encontrado           |
| 422         | Validación fallida      |
| 429         | Límite de tasa excedido |
| 500         | Error interno           |

## Paginación, filtrado y ordenamiento

- **Paginación**: `?page=1&per_page=20` (o cursor: `?cursor=...`).
- **Filtrado**: `?filtro[campo]=valor`.
- **Ordenamiento**: `?sort=-created_at`.

## Endpoints

No hay endpoints de API en esta versión. La aplicación funciona con **rutas web
server-rendered** de Django. Para referencia, las rutas web actuales son:

```text
/                       # Lista de tareas (task-list)
/tareas/                # Lista de tareas (task-list)
/tareas/nueva/          # Crear tarea (task-create)
/tareas/<pk>/editar/    # Editar tarea (task-update)
/tareas/<pk>/eliminar/  # Eliminar tarea (task-delete)
/registro/              # Registro de usuario
/accounts/...           # Auth de Django (login, logout, password_*)
/admin/                 # Panel de administración de Django
```

Estas rutas devuelven HTML, no JSON, y se consumen desde el navegador mediante
formularios. Cuando se implemente la API REST del roadmap, aquí se documentará el
recurso `Task` (listar/obtener/crear/actualizar/eliminar).

## Rate limiting

- No aplica en esta versión (no hay API pública). Se definirá junto con la futura API REST.

## Webhooks (si aplica)

- No aplica: el proyecto no expone ni consume webhooks en esta versión.

## Changelog de la API

- Sin cambios de API que registrar: el proyecto aún no expone una API en esta versión.
