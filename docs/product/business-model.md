# Gestor de Tareas — Modelo del Proyecto

> Marco: **Lean Canvas**, adaptado a un proyecto **educativo y open-source** sin
> ánimo de lucro. Describe por qué existe el proyecto y cómo genera valor.
> **Última actualización**: 2026-07-02

> **Nota importante**: Gestor de Tareas **no es un producto comercial**. Es un
> proyecto educativo y de portafolio. No tiene planes de pago, precios ni ingresos.
> Las secciones de "negocio" se reencuadran como propósito de aprendizaje y aporte
> a la comunidad.

## 1. Problema

- Aprender Django desde cero es difícil cuando la teoría no se acompaña de un
  proyecto real, completo y bien documentado.
- Muchos ejemplos disponibles están incompletos, desactualizados o mezclan
  demasiados conceptos a la vez.
- **Alternativas actuales**: tutoriales dispersos, documentación oficial (excelente
  pero densa para principiantes) y proyectos de ejemplo sin guía paso a paso.

## 2. Segmentos de "usuario"

| Segmento                    | Descripción                                                    | Notas                                    |
| --------------------------- | ------------------------------------------------------------- | ---------------------------------------- |
| Estudiantes de Django       | Personas que empiezan con Django y quieren un ejemplo guiado. | Early adopters; público principal.       |
| Desarrolladores autodidactas | Quienes buscan código de referencia para consultar patrones.  | Usan el repo como base o inspiración.    |

- **Usuario ideal**: alguien con nociones básicas de Python que quiere construir su
  primera app web con Django siguiendo una guía clara.
- **Early adopters**: estudiantes que siguen el tutorial (`docs/tutorial.md`) paso a paso.

## 3. Propuesta de valor única

- Un proyecto Django real, pequeño y completo, acompañado de una guía paso a paso
  para aprender construyendo, no solo leyendo.
- **Concepto de alto nivel**: "un proyecto de referencia para aprender Django, como
  un cuaderno de ejercicios resuelto".

## 4. Solución (el producto)

| Módulo / Capacidad | Qué resuelve                                                              |
| ------------------ | ------------------------------------------------------------------------ |
| Usuarios           | Enseña autenticación en Django: registro, login y logout.                |
| Tareas             | Enseña el CRUD por usuario y el modelado de datos (modelo `Task`, categorías). |

## 5. Canales

- Repositorio público en GitHub: <https://github.com/brayandiazc/gestortareas-django>.
- Guía paso a paso incluida en la documentación (`docs/tutorial.md`).
- Boca a boca en comunidades de aprendizaje y como pieza de portafolio del autor.

## 6. Modelo de "ingresos"

| Plan               | Precio               | Qué incluye                                            |
| ------------------ | -------------------- | ------------------------------------------------------ |
| Uso libre (único)  | Gratuito (open-source, MIT) | Acceso completo al código, la documentación y el tutorial. |

- **Lógica de facturación**: no aplica. El proyecto es gratuito y de código abierto
  bajo licencia MIT. No hay planes de pago ni suscripciones.

## 7. Estructura de costos y "rentabilidad"

- **Costos variables**: ninguno relevante; se ejecuta localmente para aprender.
- **Costos fijos**: hospedaje del repositorio (GitHub, gratuito) y el tiempo del autor.
- **Retorno esperado**: aprendizaje, práctica y valor de portafolio, no económico.

## 8. Métricas de éxito (no comerciales)

- Personas que completan el tutorial y logran ejecutar el proyecto.
- Estrellas, forks y contribuciones en GitHub.
- Claridad y utilidad percibida de la documentación.

## 9. Ventaja diferencial

- Combinación de código real y funcional con una guía didáctica paso a paso, mantenida
  y con historial de decisiones documentado (ver [ADRs](../decisions/README.md)).

## Decisiones pendientes

- [ ] Definir si se publica el tutorial también como serie de artículos o entradas de blog.
- [ ] Decidir el alcance de la v1.0 (API REST y despliegue) descrito en el [roadmap](roadmap.md).
