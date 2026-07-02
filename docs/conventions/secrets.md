# Convenciones de secretos y credenciales

> Cómo gestionamos secretos en Gestor de Tareas.
> **Última actualización**: 2026-07-02

## Filosofía

- Los secretos **nunca** se commitean en texto plano.
- Separación clara entre **configuración** (no sensible) y **secretos** (sensible).

## Dónde vive cada cosa

Los secretos se gestionan con **variables de entorno** cargadas desde un archivo
`.env` mediante `python-dotenv`. Las variables usadas son `SECRET_KEY`, `DEBUG`,
`ALLOWED_HOSTS` y las de base de datos `DB_*` (`DB_ENGINE`, `DB_NAME`, `DB_USER`,
`DB_PASSWORD`, `DB_HOST`, `DB_PORT`). Están documentadas en `.env.example`.

| Tipo                         | Dónde                                         |
| ---------------------------- | --------------------------------------------- |
| Secretos de aplicación       | Archivo `.env` local (cargado con `python-dotenv`); plantilla en `.env.example` |
| Variables de infraestructura | Variables de entorno del entorno de despliegue (por definir; ver [`deploy.md`](deploy.md)) |
| Secretos de CI/CD            | Secrets del proveedor (p. ej. GitHub Actions) — no configurado aún |

## Reglas

- El archivo `.env` está en `.gitignore`; solo se versiona `.env.example` (sin valores).
- Comparte secretos con nuevos colaboradores **fuera de banda** (nunca por git, email plano ni chat público).
- Rota credenciales periódicamente (sugerido cada 90 días) y de inmediato ante sospecha de fuga.
- Si un secreto se commitea por error: **rota el secreto primero**, luego limpia la historia.

## Ejemplos

```bash
# Copiar la plantilla de variables
cp .env.example .env
# Completar valores reales (que nunca se suben)
```

## Comandos útiles

```bash
# Copiar la plantilla de variables de entorno
cp .env.example .env

# Generar una SECRET_KEY nueva para Django
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"

# Verificar que .env está ignorado por git
git check-ignore .env
```

## Referencias

- [SECURITY.md](../../SECURITY.md) — política de seguridad.
- [python-dotenv](https://pypi.org/project/python-dotenv/)
- [Configuración de Django (`settings`)](https://docs.djangoproject.com/en/5.1/topics/settings/)
