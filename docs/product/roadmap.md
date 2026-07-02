# Roadmap — Gestor de Tareas

> Estado y dirección del producto. Documento vivo.
> **Última actualización**: 2026-07-02

## Leyenda

- ✅ Hecho
- 🚧 En curso
- 📋 Planificado
- ⏸️ Diferido

## Visión

Gestor de Tareas es un proyecto educativo y de portafolio cuyo objetivo es servir
como referencia clara y bien documentada para aprender a construir una aplicación
web con Django. A largo plazo busca cubrir, paso a paso, el flujo completo de una
app server-rendered (autenticación, CRUD por usuario, panel admin) y crecer con
funcionalidades y prácticas habituales de la industria (API, pruebas, CI y
despliegue), sin convertirse en un producto comercial.

## Estado actual

La aplicación es funcional para uso educativo. Está construida con Django 5.1.2,
Python 3.10+, PostgreSQL y Bootstrap 5, y se organiza en dos apps: `usuarios`
(registro, login y logout) y `tareas` (CRUD de tareas por usuario). El modelo
`Task` incluye `title`, `description`, `category`, `user` (FK a `User`) y
`created_at`. Se acompaña de una guía paso a paso en [`../tutorial.md`](../tutorial.md).

## Por versión / fase

### v0.1 — Base funcional (✅ Hecho)

- [x] Configuración inicial del proyecto Django con PostgreSQL y Bootstrap 5.
- [x] Autenticación de usuarios: registro, login y logout (app `usuarios`).
- [x] CRUD de tareas por usuario con categorías (app `tareas`).
- [x] Panel de administración de Django.
- [x] Guía paso a paso (`docs/tutorial.md`).

### v0.2 — Mejoras de gestión de tareas (📋 Planificado)

- [ ] Estado de tareas: marcar como completadas / pendientes.
- [ ] Fechas de vencimiento y ordenación por fecha.
- [ ] Filtros y búsqueda por categoría y estado.

### v0.3 — Calidad y automatización (📋 Planificado)

- [ ] Tests automatizados (unitarios y de vistas).
- [ ] Integración continua (CI) que ejecute las pruebas en cada cambio.

### v1.0 — API y despliegue (📋 Planificado)

- [ ] API REST (p. ej. con Django REST Framework) para exponer las tareas.
- [ ] Despliegue en un entorno accesible públicamente.

## Backlog / ideas sin agendar

- Etiquetas además de categorías.
- Priorización de tareas.
- Notificaciones o recordatorios de vencimiento.
- Internacionalización (i18n).

## Fuera de alcance

- Monetización, planes de pago o funcionalidades comerciales: es un proyecto
  educativo y open-source.
- Aplicaciones móviles nativas: el foco es la web server-rendered con Django.

## Cómo se actualiza este documento

- Revisar al cerrar cada versión/fase.
- Las decisiones que cambian el rumbo se registran como ADRs en [`../decisions/`](../decisions/README.md).
