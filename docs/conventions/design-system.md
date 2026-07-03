# Convenciones del sistema de diseño

> Tokens, componentes y reglas de UI de Gestor de Tareas.
> Para el diseño técnico/UX del producto ver
> [`../architecture/design.md`](../architecture/design.md).
> **Última actualización**: 2026-07-02

> **Estado actual**: no existe un sistema de diseño propio. Se usa **Bootstrap 5**
> por defecto (cargado por CDN), con sus componentes, utilidades y variables por
> defecto. Este documento describe cómo usarlo de forma consistente.

## Stack

- **Librería de componentes**: Bootstrap 5 (CSS + JS por CDN).
- **Solución de estilos**: clases utilitarias y componentes de Bootstrap; CSS
  propio mínimo solo cuando Bootstrap no lo cubre.
- **Página de referencia viva** (si aplica): no aplica; se usa la
  [documentación oficial de Bootstrap](https://getbootstrap.com/docs/5.3/) como referencia.

## Tokens

Se usan las variables por defecto de Bootstrap 5 (no hay tokens personalizados):

| Token          | Uso                             |
| -------------- | ------------------------------- |
| Colores        | Clases contextuales de Bootstrap: `primary`, `secondary`, `success`, `danger`, `warning`, `info` |
| Tipografía     | Familia de sistema por defecto de Bootstrap; escala `h1`–`h6`, `.fs-*` |
| Espaciado      | Escala de utilidades de Bootstrap (`m-*`, `p-*`, de 0 a 5) |
| Bordes/sombras | Utilidades `rounded`, `border`, `shadow-*` de Bootstrap |

> Usa las **clases semánticas** de Bootstrap (p. ej. `btn-primary`, `text-danger`),
> no colores hex crudos, en las plantillas.

## Componentes permitidos

- Usa los componentes de Bootstrap (botones, formularios, cards, alerts, navbar,
  modales); evita crear variantes ad-hoc.
- Los mensajes de éxito/error se muestran con el componente `alert` de Bootstrap
  a partir del framework de mensajes de Django (ver [`views-and-layouts.md`](views-and-layouts.md)).
- Cada componente debe contemplar sus **estados**: normal, hover, foco,
  deshabilitado y error de validación.

## Accesibilidad (baseline)

- Contraste mínimo objetivo: WCAG AA.
- Foco visible y navegación por teclado (Bootstrap lo provee por defecto; no lo
  anules).
- Usa etiquetas `<label>` asociadas a cada campo de formulario y los
  roles/atributos ARIA que Bootstrap recomienda para cada componente.

## Anti-patrones

- Colores hex o espaciados en píxeles hardcodeados en las plantillas en lugar de
  las clases utilitarias de Bootstrap.
- CSS propio que reimplementa un componente que Bootstrap ya ofrece.
- Cargar otra librería de CSS en paralelo que compita con Bootstrap.

## Referencias

- [Documentación de Bootstrap 5](https://getbootstrap.com/docs/5.3/)
- [`branding.md`](branding.md)
- [`views-and-layouts.md`](views-and-layouts.md)
