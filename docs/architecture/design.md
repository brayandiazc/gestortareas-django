# Diseño — Gestor de Tareas

> Decisiones de diseño técnico y de producto: cómo se resuelve el problema y
> cómo se ve y se siente el producto. Las decisiones relevantes se promueven a
> ADRs en [`../decisions/`](../decisions/README.md).
>
> **Última actualización**: 2026-07-02

## Contexto y objetivos

- **Problema**: Los usuarios necesitan una forma simple de crear, organizar y consultar sus tareas personales, con cada usuario viendo únicamente las suyas.
- **Objetivos**: Ofrecer un CRUD de tareas claro y responsive, con autenticación y aislamiento de datos por usuario, sin complejidad de frontend adicional.
- **No-objetivos**: No se busca exponer una API REST, ni una SPA con framework JS, ni internacionalización (la interfaz es en español fijo) en esta versión.

## Requisitos

### Funcionales

- Registro e inicio/cierre de sesión de usuarios.
- CRUD de tareas (crear, listar, editar y eliminar) restringido a las tareas del usuario autenticado.
- Cada tarea tiene título, descripción opcional, categoría y fecha de creación; se listan de la más reciente a la más antigua.

### No funcionales

- **Seguridad**: acceso a tareas solo con sesión iniciada y filtrado por `request.user`; contraseñas hasheadas por Django.
- **Simplicidad / mantenibilidad**: monolito Django con renderizado en servidor, sin build de frontend.
- **Responsividad**: interfaz adaptable a móvil y escritorio mediante el grid de Bootstrap 5.

## Diseño propuesto

- **Enfoque general**: Aplicación Django monolítica con patrón MTV. Vistas basadas en clase renderizan plantillas de Django estilizadas con Bootstrap 5.
- **Componentes y flujos**: Las URLs enrutan a vistas que consultan el ORM y renderizan plantillas. Ver [`architecture.md`](architecture.md) para el detalle de componentes y flujos.

## Alternativas consideradas

| Alternativa                | Pros                                   | Contras                                  | ¿Por qué se descartó?                          |
| -------------------------- | -------------------------------------- | ---------------------------------------- | ---------------------------------------------- |
| SPA (React/Vue) + API REST | UI más rica e interactiva              | Mayor complejidad, dos bases de código   | Excesivo para un CRUD simple; SSR es suficiente. |
| CSS propio en lugar de Bootstrap | Control total del estilo         | Más tiempo de desarrollo y mantenimiento | Bootstrap vía CDN entrega componentes listos.  |

## Identidad visual y sistema de diseño

- **Principios de diseño**: Claridad, consistencia y simplicidad; priorizar la legibilidad de la lista de tareas.
- **Tokens**: Se apoyan en el sistema por defecto de Bootstrap 5 (colores, tipografía y espaciado de utilidades de Bootstrap).
- **Componentes**: Componentes de Bootstrap 5 (navbar, cards, formularios, botones, alerts) cargados vía CDN en la plantilla base `base_generic.html`. La navbar es dinámica según el estado de autenticación (muestra login/registro o el usuario y logout). Los mensajes flash de Django (`django.contrib.messages`) se renderizan como alerts de Bootstrap.

## Accesibilidad

- Contraste de color (objetivo WCAG AA), apoyado en los colores por defecto de Bootstrap.
- Navegación por teclado y foco visible.
- Roles/atributos ARIA donde corresponda.

## Estados de la interfaz

Cada vista debe contemplar: **carga**, **vacío**, **error** y **éxito**.

## Modelo de datos afectado

Enlaza a [`database.md`](database.md) si el diseño introduce o cambia entidades.

## Riesgos y mitigaciones

| Riesgo                                            | Impacto | Mitigación                                                        |
| ------------------------------------------------- | ------- | ----------------------------------------------------------------- |
| Fuga de tareas entre usuarios por vista mal filtrada | Alto    | Todas las vistas de tareas usan `LoginRequiredMixin` y filtran el queryset por `request.user`. |
| Dependencia del CDN de Bootstrap (sin build local) | Bajo    | Bootstrap es estable y ampliamente cacheado; se puede migrar a estáticos locales si hace falta. |

## Preguntas abiertas

- [ ] ¿Se añadirá un estado de completado (booleano) a las tareas en próximas versiones?
- [ ] ¿Se expondrá una API REST para clientes externos (ver roadmap)?
