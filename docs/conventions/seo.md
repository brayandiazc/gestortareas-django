# Convenciones de SEO

> Metadatos y buenas prácticas de SEO en Gestor de Tareas.
> **Última actualización**: 2026-07-02

> **Estado actual: no prioritario.** Gestor de Tareas es una aplicación de acceso
> **autenticado**, sin páginas públicas de marketing, por lo que el SEO es mínimo:
> todas las vistas relevantes deben marcarse `noindex`. Esta guía queda como
> referencia por si se añaden páginas públicas (landing, blog) en el futuro.

## Qué emite el head compartido

Cada página debe proveer, a través del layout/head compartido:

- `<title>` único y descriptivo.
- `<meta name="description">`.
- Open Graph: `og:title`, `og:description`, `og:image`, `og:url`, `og:type`.
- Twitter Card: `twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`.
- `<link rel="canonical">`.
- `<meta name="robots">` según la página.

## Reglas de indexación

| Tipo de página                 | robots          |
| ------------------------------ | --------------- |
| Públicas (landing, blog)       | `index, follow` |
| Autenticación (login/registro) | `noindex`       |
| Áreas privadas (dashboard)     | `noindex`       |

## Reglas

- Una sola etiqueta canónica por página.
- Imagen OG con dimensiones recomendadas (1200×630).
- URLs legibles y estables; evitar parámetros innecesarios.
- `sitemap.xml` y `robots.txt` mantenidos al día.

## Ejemplos

En `base_generic.html`, dado que la app es de acceso autenticado, el head debería
marcar las vistas como no indexables:

```html
<meta name="robots" content="noindex" />
<meta name="description" content="Gestor de Tareas — organiza tus tareas." />
```

## Referencias

- [Etiqueta `robots` (documentación de buscadores)](https://developers.google.com/search/docs/crawling-indexing/robots-meta-tag)
- [`views-and-layouts.md`](views-and-layouts.md)
