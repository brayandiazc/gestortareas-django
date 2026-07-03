# Convenciones de testing

> Cómo escribimos y ejecutamos tests en Gestor de Tareas.
> **Última actualización**: 2026-07-02

## Stack

- **Framework de tests**: el runner de tests de Django, basado en `unittest`
  (`django.test.TestCase`), ejecutado con `python manage.py test`.
- **Cobertura**: no configurada aún (**propuesto**: [coverage.py](https://coverage.readthedocs.io/)).
- **Tests de sistema/E2E**: no configurados (propuesto a futuro: Django
  `LiveServerTestCase` + Selenium/Playwright si el proyecto lo requiere).

## Tipos de test

Los tests viven en el archivo `tests.py` de cada app. Django no separa por
carpetas por defecto; para suites grandes puede convertirse `tests.py` en un
paquete `tests/`.

| Tipo          | Qué cubre                     | Carpeta              |
| ------------- | ----------------------------- | -------------------- |
| Unitarios     | Modelos, formularios y utilidades aislados | `tareas/tests.py`, `usuarios/tests.py` |
| Integración   | Vistas + URLs + plantillas (vía `Client`)  | `tareas/tests.py`, `usuarios/tests.py` |
| E2E / sistema | Flujos completos de usuario   | no configurado aún   |

## Reglas

- Todo cambio funcional se acompaña de tests.
- Estructura **Arrange-Act-Assert** (AAA): preparar, ejecutar, verificar.
- Un test verifica **una** cosa; nombres descriptivos del comportamiento esperado.
- Los tests deben ser deterministas (sin dependencia de red, reloj o orden).
- Cada test corre sobre una base de datos de prueba aislada que Django crea y
  destruye automáticamente.
- Cobertura mínima esperada: no definida aún (**propuesta**: 70% como objetivo
  inicial, una vez se configure coverage.py).

## Ejemplos

```python
from django.test import TestCase
from django.contrib.auth.models import User
from tareas.models import Task


class TaskModelTest(TestCase):
    def test_str_devuelve_el_titulo_cuando_se_crea_una_tarea(self):
        # Arrange
        user = User.objects.create_user(username="ana", password="secreta123")
        # Act
        tarea = Task.objects.create(title="Comprar pan", category="hogar", user=user)
        # Assert
        self.assertEqual(str(tarea), "Comprar pan")
```

## Comandos útiles

```bash
python manage.py test                 # Ejecutar todos los tests
python manage.py test tareas          # Ejecutar los tests de una app
python manage.py test --verbosity 2   # Salida detallada

# Con cobertura (propuesto — requiere: pip install coverage)
coverage run manage.py test
coverage report        # resumen en consola
coverage html          # reporte navegable en htmlcov/
```

> No hay un modo *watch* nativo en Django; se re-ejecuta `manage.py test` tras
> cada cambio (o se usa una herramienta externa como `pytest-watch` si se adopta
> pytest en el futuro).

## Referencias

- [Testing en Django](https://docs.djangoproject.com/en/5.1/topics/testing/)
- [coverage.py](https://coverage.readthedocs.io/) (propuesto)
