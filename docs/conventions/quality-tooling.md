# Convenciones de calidad y tooling

> Linters, formato, análisis estático y git hooks de Gestor de Tareas.
> **Última actualización**: 2026-07-02

> **Estado actual**: no hay linters ni formateadores configurados formalmente
> todavía. Sí existe un archivo `.editorconfig` en la raíz del repo que fija
> reglas básicas de estilo (indentación, fin de línea, charset). Las herramientas
> de esta sección están **recomendadas / propuestas**, no impuestas aún.

## Stack

- **Linter**: recomendado [flake8](https://flake8.pycqa.org/) o
  [Ruff](https://docs.astral.sh/ruff/) (Ruff cubre linter y formato a la vez).
- **Formateador**: recomendado [Black](https://black.readthedocs.io/) (o el
  formateador de Ruff).
- **Análisis estático / seguridad**: recomendado [Bandit](https://bandit.readthedocs.io/)
  y `python manage.py check --deploy` para revisar la configuración de despliegue.
- **Auditoría de dependencias**: recomendado [pip-audit](https://pypi.org/project/pip-audit/).
- **Orquestador de git hooks**: recomendado [pre-commit](https://pre-commit.com/)
  (opcional; aún no configurado).
- **EditorConfig**: `.editorconfig` (ya presente) — respetado por la mayoría de
  editores para estilo básico consistente.

## Git hooks

Estrategia sugerida: hooks baratos y rápidos en `pre-commit`, los más costosos en
`pre-push`. CI ejecuta todo de nuevo en el servidor.

### pre-commit (en cada commit)

- Linter sobre archivos cambiados.
- Formato automático.
- Verificación de trailing whitespace, fin de archivo, conflictos sin resolver.

### pre-push (al subir)

- Linter completo.
- Tests (o un subconjunto rápido).
- Auditoría de dependencias.

## Reglas

- El código debe pasar linter y formato antes del merge (una vez configurados).
- Los checks de calidad deberían ser **bloqueantes** en CI (CI aún no configurado;
  ver [`deploy.md`](deploy.md)).
- Respetar `.editorconfig` en todos los editores.

## Comandos útiles

```bash
# Formato (recomendado — requiere: pip install black)
black .

# Linter (recomendado — requiere: pip install flake8   o   pip install ruff)
flake8 .
ruff check .

# Chequeos propios de Django
python manage.py check
python manage.py check --deploy      # revisiones de seguridad para producción

# Auditoría de dependencias (recomendado — requiere: pip install pip-audit)
pip-audit
```

## Referencias

- [Black](https://black.readthedocs.io/) · [Ruff](https://docs.astral.sh/ruff/) ·
  [flake8](https://flake8.pycqa.org/)
- [pre-commit](https://pre-commit.com/) · [pip-audit](https://pypi.org/project/pip-audit/)
- [EditorConfig](https://editorconfig.org/)
