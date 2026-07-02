# Convenciones de autenticación y autorización

> Reglas transversales de autenticación y autorización en Gestor de Tareas.
> Para cómo funciona la auth en este proyecto ver
> [`../architecture/auth.md`](../architecture/auth.md).
> **Última actualización**: 2026-07-02

## Stack

- **Autenticación**: sistema de autenticación de Django (`django.contrib.auth`),
  con sesiones de Django (cookie de sesión respaldada en servidor).
- **Autorización**: protección de vistas con `LoginRequiredMixin` (vistas basadas
  en clases); cada usuario solo accede a sus propios registros filtrando el
  queryset por `request.user`.
- **Hashing de contraseñas**: hashers por defecto de Django (PBKDF2 con SHA256 y
  salt). Las contraseñas se validan con `AUTH_PASSWORD_VALIDATORS`.

## Reglas

- La autorización se valida **siempre en el servidor**, en cada request
  (`LoginRequiredMixin` en las vistas protegidas).
- Nunca confiar en checks de cliente para decisiones de seguridad.
- Las contraseñas se almacenan hasheadas por Django (PBKDF2 + salt); nunca en
  texto plano.
- Las contraseñas nuevas pasan por `AUTH_PASSWORD_VALIDATORS` (longitud mínima,
  similitud con datos del usuario, contraseñas comunes, solo-numéricas).
- Las sesiones las gestiona Django; se renuevan al iniciar sesión.
- Un usuario solo puede ver y modificar **sus propias** tareas: los querysets se
  filtran por `request.user` (p. ej. `Task.objects.filter(user=self.request.user)`).
- OAuth/SSO: no aplica actualmente (solo usuario/contraseña).

## Modelo

- **Usuario**: modelo `User` estándar de Django (`django.contrib.auth.models.User`);
  campos clave `username`, `password` (hasheado), `email`.
- **Sesión**: cookie de sesión de Django respaldada por la tabla de sesiones en la
  base de datos. No se usan tokens de API.
- **Roles / permisos**: no hay RBAC/ABAC propio; la única regla de acceso es
  "usuario autenticado dueño del recurso". Existe el sistema de permisos/grupos de
  Django si se necesitara a futuro.

## Ejemplos

```python
# Vista protegida: requiere sesión y limita a las tareas del usuario
from django.contrib.auth.mixins import LoginRequiredMixin
from django.views.generic import ListView
from .models import Task


class TaskListView(LoginRequiredMixin, ListView):
    model = Task

    def get_queryset(self):
        return Task.objects.filter(user=self.request.user)
```

## Comandos útiles

```bash
python manage.py createsuperuser          # crear un usuario administrador
python manage.py changepassword <user>    # cambiar la contraseña de un usuario
```

## Referencias

- [Autenticación en Django](https://docs.djangoproject.com/en/5.1/topics/auth/)
- [`../architecture/auth.md`](../architecture/auth.md)
- [SECURITY.md](../../SECURITY.md)
