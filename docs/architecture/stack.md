# Stack Tecnológico

> Fuente de verdad de las tecnologías y versiones del proyecto.
> **Última actualización**: 2026-07-02

## Frontend

| Categoría | Tecnología            | Versión | Por qué |
| --------- | --------------------- | ------- | ------- |
| Framework | Plantillas de Django (server-side rendering) | 5.1.2 | No se usa framework JS; las vistas se renderizan en el servidor con el motor de plantillas de Django, manteniendo la app simple. |
| Estado    | No aplica (sin estado en cliente) | —       | Al ser SSR, el estado vive en el servidor y en la base de datos; no hay librería de estado en cliente. |
| Estilos   | Bootstrap 5 (vía CDN) | 5       | Ofrece componentes y grid responsive listos para usar sin pipeline de build de CSS. |
| Build     | No aplica (sin bundler) | —       | Los estáticos se sirven desde CDN y `staticfiles` de Django; no hay paso de bundling. |

## Backend

| Categoría           | Tecnología       | Versión | Por qué |
| ------------------- | ---------------- | ------- | ------- |
| Runtime             | Python           | 3.10+   | Versión moderna compatible con Django 5.x y con anotaciones de tipo actuales. |
| Framework           | Django           | 5.1.2   | Framework web "baterías incluidas" con ORM, auth y admin integrados; acelera el desarrollo de un CRUD monolítico. |
| ORM / capa de datos | Django ORM       | 5.1.2   | Incluido en Django; mapea los modelos a PostgreSQL y gestiona migraciones. |
| Validación          | Django Forms (`UserCreationForm`, `ModelForm`) | 5.1.2 | La validación de entrada se hace con los formularios y validadores del propio Django. |

## Base de Datos

| Categoría | Tecnología           | Versión | Por qué |
| --------- | -------------------- | ------- | ------- |
| Principal | PostgreSQL (driver `psycopg2-binary`) | 2.9.9 (driver) | Base de datos relacional robusta y soportada oficialmente por Django. |
| Cache     | No aplica            | —       | El proyecto no usa una capa de cache en esta versión. |
| Cola      | No aplica            | —       | No hay procesamiento asíncrono ni cola de tareas en esta versión. |

## DevOps & Herramientas

| Categoría    | Tecnología                     |
| ------------ | ------------------------------ |
| CI/CD        | No configurado en esta versión |
| Contenedores | No aplica (entorno virtual `venv`) |
| Orquestación | No aplica                      |
| Monitoreo    | No configurado                 |
| Testing      | Framework de tests de Django (`python manage.py test`) |

## Servicios externos

| Servicio               | Uso                                     | Credenciales necesarias |
| ---------------------- | --------------------------------------- | ----------------------- |
| CDN de Bootstrap 5     | Servir CSS/JS de Bootstrap en la plantilla base | Ninguna |
| python-dotenv (1.0.1)  | Carga de variables de entorno desde `.env` | Variables del archivo `.env` (`SECRET_KEY`, `DB_*`, etc.) |

## Justificación de elecciones

| Tecnología elegida | Alternativa descartada | Razón                                                   |
| ------------------ | ---------------------- | ------------------------------------------------------- |
| Django (SSR)       | API REST + SPA (React/Vue) | Para un CRUD sencillo de tareas, el renderizado en servidor reduce complejidad y no requiere frontend separado. |
| PostgreSQL         | SQLite                 | Se prefiere una base de datos de producción robusta con mejor concurrencia e integridad. |
| Bootstrap vía CDN  | CSS propio / pipeline de build | Componentes responsive listos sin herramientas de build. |

## Versiones mínimas soportadas

- Python >= 3.10
- Django >= 5.1.2
- PostgreSQL (compatible con `psycopg2-binary` 2.9.9)
