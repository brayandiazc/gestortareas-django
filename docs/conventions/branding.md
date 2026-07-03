# Convenciones de marca (branding)

> Identidad visual y gestión de assets de marca de Gestor de Tareas.
> Para tokens y componentes de UI ver [`design-system.md`](design-system.md).
> **Última actualización**: 2026-07-02

> **Estado actual**: Gestor de Tareas es un **proyecto educativo sin identidad de
> marca formal**. No hay logos, paleta ni tipografía propios: la apariencia la
> aporta Bootstrap 5 por defecto (ver [`design-system.md`](design-system.md)).
> Esta guía queda como referencia reutilizable por si se define una marca a futuro.

## Logos

No hay assets de logo definidos actualmente. Si se crean, seguir este esquema:

| Asset             | Archivo fuente     | Uso                     |
| ----------------- | ------------------ | ----------------------- |
| Isotipo (símbolo) | (por definir, p. ej. `static/branding/logo-mark.svg`) | Favicon, app icon |
| Logotipo (texto)  | (por definir, p. ej. `static/branding/logo.svg`)      | Header, materiales |
| Versión monocroma | (por definir, p. ej. `static/branding/logo-mono.svg`) | Fondos de un solo color |

- Cuando existan, mantén los fuentes en formato vectorial (SVG) y genera los
  rasterizados desde ahí.

## Paleta y tipografía

- No hay tokens de marca propios; se usan los valores por defecto de Bootstrap 5
  (ver [`design-system.md`](design-system.md)).
- **Colores de marca**: no definidos (se usa la paleta contextual de Bootstrap:
  `primary`, `secondary`, etc.).
- **Tipografías**: no definidas (se usa la *system font stack* por defecto de
  Bootstrap).

## Reglas de uso

- Respeta el área de protección (espacio libre) alrededor del logo.
- No deformes, recolores ni apliques efectos no autorizados al logo.
- Usa la variante adecuada según el fondo (claro/oscuro/color).

## Assets

No hay carpeta de assets de marca todavía. Estructura sugerida si se crea (dentro
de los archivos estáticos de Django):

```text
static/branding/         # (propuesto, no existe aún)
├── logo.svg
├── logo-mark.svg
└── og-image.png   # 1200×630 para compartir en redes
```

## Referencias

- [`design-system.md`](design-system.md)
- Guía de marca completa: no existe (proyecto educativo).
