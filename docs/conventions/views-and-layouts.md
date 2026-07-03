# Convenciones de vistas y layouts

> Cómo organizamos vistas, layouts y UI compartida en Gestor de Tareas.
> **Última actualización**: 2026-07-02

Las vistas se renderizan en el servidor con el sistema de plantillas de Django
(patrón MTV). Se usa herencia de plantillas (`{% extends %}` / `{% block %}`) a
partir de un layout base. La UI se construye con Bootstrap 5 cargado por CDN.

## Layouts

| Layout           | Archivo | Uso                                         |
| ---------------- | ------- | ------------------------------------------- |
| Base / App       | `templates/base_generic.html` | Layout base del producto autenticado; navbar responsiva y bloque de contenido |
| Autenticación    | `templates/registration/` (`login.html`, `registro.html`) | Pantallas de login y registro |

> No existe un layout público de marketing; el proyecto es una app de acceso
> autenticado (ver [`seo.md`](seo.md)). El layout base y el de auth comparten la
> misma estructura Bootstrap.

## Elementos compartidos

- **Head compartido**: `<head>` definido en `base_generic.html` (carga de
  Bootstrap por CDN, `<title>`, metadatos). SEO mínimo (ver [`seo.md`](seo.md)).
- **Mensajes flash**: framework de mensajes de Django (`django.contrib.messages`),
  renderizados como alertas de Bootstrap en el layout base.
- **Navegación**: navbar responsiva de Bootstrap en el layout base, reutilizada
  por todas las vistas autenticadas.

## Reglas

- Toda plantilla extiende `base_generic.html`; no dupliques la estructura del
  `<head>` ni de la navbar.
- Extrae marcado repetido a parciales incluidos con `{% include %}`.
- Cada vista contempla sus estados: vacío (lista sin tareas), error (validación de
  formularios) y éxito (mensaje flash tras crear/editar/eliminar).
- La UI sigue el [sistema de diseño](design-system.md) (Bootstrap 5).
- Separa estructura (layout base) de contenido (plantilla de la vista) del
  comportamiento (vistas basadas en clases en `views.py`).
- Las vistas que requieren sesión usan `LoginRequiredMixin` (ver
  [`authentication.md`](authentication.md)).

## Estructura

Las plantillas viven en el `templates/` del proyecto y en el `templates/` de cada
app (Django las descubre por `APP_DIRS`):

```text
templates/
├── base_generic.html          # layout base (navbar + bloque de contenido)
└── registration/
    ├── login.html             # inicio de sesión
    └── registro.html          # registro de usuario

tareas/templates/tareas/
├── task_list.html             # listado de tareas
├── task_form.html             # alta / edición
└── task_confirm_delete.html   # confirmación de borrado
```

## Referencias

- [Plantillas de Django](https://docs.djangoproject.com/en/5.1/topics/templates/)
- [`design-system.md`](design-system.md)
- [`seo.md`](seo.md)
