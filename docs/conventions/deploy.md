# Convenciones de despliegue

> Operaciones de producción de Gestor de Tareas. Fuente de verdad de cómo se
> despliega, se hace rollback y se opera el sistema.
> **Última actualización**: 2026-07-02

> **Estado actual**: **no hay despliegue configurado todavía**. El proyecto se
> ejecuta en desarrollo local con `python manage.py runserver` en el puerto 8000.
> Este documento describe el **procedimiento genérico recomendado** para llevar
> una aplicación Django a producción. El pipeline de CI/CD y los ambientes
> concretos aún **no** están definidos.

## Stack de infraestructura

- **Hosting / cómputo**: por definir (opciones razonables para Django: un VPS con
  Gunicorn + Nginx, o una PaaS como Railway / Render / Fly.io).
- **DNS / TLS**: por definir (se recomienda certificados gestionados, p. ej.
  Let's Encrypt vía Nginx/Certbot o el TLS automático de la PaaS).
- **Contenedores / orquestación**: no aplica actualmente (propuesto: Docker para
  empaquetar la app de forma reproducible).
- **CI/CD**: no configurado aún (propuesto: GitHub Actions; ya existe la carpeta
  `.github/`).

## Ambientes

> Ninguno de estos ambientes está aprovisionado todavía; la tabla refleja el
> esquema objetivo. Las URLs quedan **por definir**.

| Ambiente   | URL              | Rama      | Deploy     |
| ---------- | ---------------- | --------- | ---------- |
| Desarrollo | `http://localhost:8000` (local) | `develop` | Manual (local) |
| Staging    | (por definir)    | `staging` | Automático (propuesto) |
| Producción | (por definir)    | `main`    | Manual (propuesto) |

## Reglas

- Solo se despliega a producción desde `main`.
- Cada deploy debe ser reproducible y reversible.
- Las variables de entorno y secretos se gestionan según [`secrets.md`](secrets.md)
  (`SECRET_KEY`, `DEBUG`, `ALLOWED_HOSTS`, `DB_*`).
- En producción **`DEBUG=False`** y `ALLOWED_HOSTS` con los dominios reales.
- Ejecutar `migrate` y `collectstatic` en cada deploy antes de reiniciar el
  servidor de aplicación.
- Verificar que la aplicación responde tras cada deploy.

## Procedimiento de deploy

Procedimiento genérico recomendado para Django (aún no automatizado):

```bash
# 1. Obtener el código y las dependencias
git pull origin main
pip install -r requirements.txt

# 2. Preparar variables de entorno de producción (DEBUG=False, etc.)
#    Ver .env.example y docs/conventions/secrets.md

# 3. Aplicar migraciones de base de datos
python manage.py migrate --noinput

# 4. Recolectar archivos estáticos
python manage.py collectstatic --noinput

# 5. Servir con un servidor WSGI de producción (no runserver)
gunicorn gestor_tareas.wsgi:application --bind 0.0.0.0:8000

# 6. Verificar que responde
curl http://localhost:8000/
```

## Rollback

```bash
# Volver al commit/tag estable anterior y re-desplegar
git checkout <tag_o_commit_estable>
pip install -r requirements.txt
python manage.py migrate --noinput   # revertir a la migración previa si aplica:
                                     # python manage.py migrate <app> <migracion_anterior>
python manage.py collectstatic --noinput
# reiniciar el servicio de Gunicorn/WSGI
```

## Health checks y monitoreo

- Endpoint de salud: no hay endpoint dedicado todavía (propuesto: una vista
  simple que devuelva 200, p. ej. `/health/`). Como verificación mínima puede
  usarse la ruta raíz `/`.
- Monitoreo de errores: no configurado (propuesto: Sentry).
- Alertas: por definir.

## Referencias

- [Deployment checklist de Django](https://docs.djangoproject.com/en/5.1/howto/deployment/checklist/)
- [Cómo desplegar Django con WSGI/Gunicorn](https://docs.djangoproject.com/en/5.1/howto/deployment/wsgi/gunicorn/)
- [`secrets.md`](secrets.md)
