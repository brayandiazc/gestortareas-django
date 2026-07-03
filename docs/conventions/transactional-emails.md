# Convenciones de correos transaccionales

> Cómo enviamos correos transaccionales en Gestor de Tareas.
> **Última actualización**: 2026-07-02

> **Estado actual: no aplica.** No hay envío de correos transaccionales
> configurado. El proyecto no define un backend de email ni un proveedor de envío,
> y ninguna funcionalidad (registro, tareas) dispara correos. Esta guía queda como
> referencia reutilizable por si se añade envío de correos en el futuro; los
> valores concretos quedan **por definir**.

## Stack

- **Proveedor de envío**: no configurado (propuesto para producción: un proveedor
  SMTP/API como SendGrid, Mailgun o Amazon SES).
- **Entorno de desarrollo**: no configurado (propuesto: el backend de consola de
  Django, `django.core.mail.backends.console.EmailBackend`, que imprime los
  correos en la terminal sin enviarlos).

## Configuración por entorno

> Tabla objetivo; ninguna de estas configuraciones existe todavía.

| Entorno    | Mecanismo de envío       |
| ---------- | ------------------------ |
| Desarrollo | Backend de consola de Django (propuesto) |
| Test       | `locmem` — backend en memoria de Django (propuesto) |
| Producción | Proveedor SMTP/API real (por definir) |

## Reglas

- Todo correo se envía en **HTML y texto plano** (multipart).
- Operaciones idempotentes: registrar `*_enviado_at` para no reenviar.
- No incluir secretos ni PII innecesaria en el cuerpo.
- Asuntos y contenido localizados (ver [`i18n.md`](i18n.md)).
- Enlaces con URLs absolutas y firmadas/expirables cuando corresponda.

## Tipos de correo

Ninguno implementado. Candidatos si se añade envío de correos a futuro:

| Correo           | Disparador          |
| ---------------- | ------------------- |
| Bienvenida       | Registro de usuario (propuesto) |
| Recuperar acceso | Solicitud de reset de contraseña (propuesto; Django ofrece este flujo listo) |

## Ejemplos

```text
Asunto: Bienvenido a Gestor de Tareas
Cuerpo: HTML + texto plano, con CTA y enlace de verificación
```

## Referencias

- [Envío de email en Django](https://docs.djangoproject.com/en/5.1/topics/email/)
- [`i18n.md`](i18n.md)
