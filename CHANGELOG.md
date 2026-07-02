# Changelog

Todos los cambios notables de este proyecto se documentan en este archivo.

El formato se basa en [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/)
y este proyecto adhiere a [Semantic Versioning](https://semver.org/lang/es/).

## [Unreleased]

### Added

### Changed

### Deprecated

### Removed

### Fixed

### Security

## [1.0.0] - 2026-07-02

### Added

- Gestión de tareas (CRUD): crear, listar, editar y eliminar tareas por usuario,
  con categorías y ordenación por fecha de creación.
- Autenticación de usuarios con el sistema integrado de Django: registro, inicio
  y cierre de sesión. Cada usuario sólo accede a sus propias tareas
  (`LoginRequiredMixin` + filtrado de `queryset`).
- Interfaz responsiva con Bootstrap 5 y plantillas de Django.
- Documentación completa del proyecto adoptando la plantilla
  [project-starter-template-es](https://github.com/brayandiazc/project-starter-template-es):
  arquitectura, stack, base de datos, autenticación, convenciones, decisiones
  (ADR), roadmap y guía paso a paso (`docs/tutorial.md`).
- Archivos de gobernanza: `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`,
  `LICENSE` (MIT), `CHANGELOG.md`, plantillas de issues/PR y configuración de
  Dependabot.
- `.env.example`, `.gitignore` y `requirements.txt` para reproducir el entorno.
- Workflow de CI de ejemplo para Django (`.github/workflows/ci.example.yml`).

### Changed

- La configuración de Django (`settings.py`) ahora lee `SECRET_KEY`, `DEBUG`,
  `ALLOWED_HOSTS` y las credenciales de base de datos desde variables de entorno
  (`python-dotenv`) en lugar de valores hardcodeados.
- El `README.md` pasó de ser un tutorial a una portada del proyecto; el contenido
  paso a paso se movió a `docs/tutorial.md`.

### Security

- Se eliminaron del código la `SECRET_KEY` y la contraseña de base de datos que
  estaban hardcodeadas en `settings.py`; ahora se gestionan como secretos vía
  `.env` (ver `SECURITY.md` y `docs/conventions/secrets.md`).

<!--
Enlaces de comparación entre versiones:
[Unreleased]: https://github.com/brayandiazc/gestortareas-django/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/brayandiazc/gestortareas-django/releases/tag/v1.0.0
-->
