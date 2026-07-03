# GESTOR DE TAREAS

```
gestor_tareas/
├── gestor_tareas/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
├── tareas/
│   ├── migrations/
│   │   ├── __init__.py
│   │   └── ...
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── forms.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
├── usuarios/
│   ├── migrations/
│   │   ├── __init__.py
│   │   └── ...
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── forms.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
├── templates/
│   ├── base_generic.html
│   └── registration/
│       ├── login.html
│       └── registro.html
├── manage.py
└── ... (otros archivos y carpetas)
```

## PASO A PASO

### Paso 1: Crear un entorno virtual y configurar Django

1. **Crear el entorno virtual**:

Abre una terminal en la carpeta donde deseas crear el proyecto y ejecuta:

```bash
python3 -m venv gestor_tareas_env
```

Esto creará un entorno virtual llamado `gestor_tareas_env`.

2. **Activar el entorno virtual**:

- En Linux/macOS:

```bash
source gestor_tareas_env/bin/activate
```

- En Windows:

```bash
gestor_tareas_env\Scripts\activate
```

1. **Instalar Django**:

Con el entorno virtual activado, instala Django:

```bash
pip install django
```

2. **Crear el proyecto de Django**:

Ahora vamos a crear el proyecto. En la terminal, ejecuta:

```bash
django-admin startproject gestor_tareas
cd gestor_tareas
```

### Paso 2: Configurar PostgreSQL

1. **Instalar psycopg2**:

Django necesita `psycopg2` para conectarse a PostgreSQL, así que instálalo con:

```bash
pip install psycopg2-binary
```

2. **Modificar el archivo `settings.py`**:

Abre el archivo `settings.py` y busca la configuración de `DATABASES`. Reemplázala con la siguiente para conectarte a PostgreSQL:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'gestor_tareas_db',
        'USER': 'gestor_user',
        'PASSWORD': 'tu_contraseña_segura',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

3. **Crear la base de datos en PostgreSQL**:

Abre una terminal y ejecuta el siguiente comando para ingresar al shell de PostgreSQL:

```bash
sudo -u postgres psql
```

Dentro del shell de PostgreSQL, crea la base de datos y el usuario con los siguientes comandos:

```sql
CREATE DATABASE gestor_tareas_db;
CREATE USER gestor_user WITH PASSWORD 'tu_contraseña_segura';
ALTER ROLE gestor_user SET client_encoding TO 'utf8';
ALTER ROLE gestor_user SET default_transaction_isolation TO 'read committed';
ALTER ROLE gestor_user SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE gestor_tareas_db TO gestor_user;
```

Luego, escribe `\q` para salir del shell.

### Paso 3: Aplicar migraciones iniciales en Django

1. **Aplicar las migraciones iniciales**:

Django necesita crear sus tablas predeterminadas para la autenticación y otras funcionalidades básicas. Ejecuta:

```bash
python manage.py migrate
```

2. **Iniciar el servidor de desarrollo**:

Asegúrate de que todo está funcionando bien. Inicia el servidor:

```bash
python manage.py runserver
```

Ve a tu navegador y accede a `http://127.0.0.1:8000`. Si ves la página de bienvenida de Django, ¡todo está funcionando correctamente!

### Paso 4: Configuración de autenticación de usuarios

#### 4.1 Configurar URLs y vistas para la autenticación

Django ya incluye un sistema de autenticación listo para usar. Usaremos las vistas predeterminadas para manejar el registro, inicio de sesión y cierre de sesión.

1. **Crear una nueva aplicación para gestionar usuarios**:

Primero, vamos a crear una aplicación para manejar todo lo relacionado con usuarios. En tu terminal, ejecuta:

```bash
python manage.py startapp usuarios
```

2. **Añadir la aplicación al proyecto**:

Abre el archivo `settings.py` y añade la nueva aplicación `usuarios` en la lista `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    ...
    'usuarios',
    'django.contrib.auth',
    'django.contrib.messages',
]
```

3. **Configurar las URLs de autenticación**:

En el archivo `urls.py` del proyecto principal (`gestor_tareas/urls.py`), añade las rutas para manejar la autenticación:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('django.contrib.auth.urls')),  # Añadimos las URLs de autenticación
    path('', include('usuarios.urls')),  # Nuestra app de usuarios
]
```

Las URLs de autenticación que añade Django incluyen:

- `/accounts/login/`: Página de inicio de sesión.
- `/accounts/logout/`: Página de cierre de sesión.
- `/accounts/password_change/`: Cambio de contraseña.

4. **Crear URLs para la app de usuarios**:

Crea un archivo `urls.py` dentro de la carpeta `usuarios/` y añade las siguientes rutas:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('registro/', views.Registro.as_view(), name='registro'),
]
```

#### 4.2 Crear la vista para el registro de usuarios

1. **Crear la vista basada en clases para el registro**:

Dentro del archivo `views.py` de la aplicación `usuarios`, vamos a crear una vista para registrar nuevos usuarios usando el formulario predeterminado de Django (`UserCreationForm`):

```python
from django.urls import reverse_lazy
from django.views import generic
from django.contrib.auth.forms import UserCreationForm

class Registro(generic.CreateView):
    form_class = UserCreationForm
    success_url = reverse_lazy('login')  # Redirige al login después del registro
    template_name = 'registration/registro.html'
```

2. **Crear la plantilla para el registro**:

Crea un directorio llamado `templates` dentro de la carpeta `usuarios` y dentro de él un subdirectorio `registration`. Luego, dentro de este directorio, crea el archivo `registro.html` con el siguiente contenido:

```html
{% extends 'base_generic.html' %} {% block content %}
<div class="container mt-4">
  <h2>Registro de Usuario</h2>
  <form method="post">
    {% csrf_token %} {{ form.as_p }}
    <button type="submit" class="btn btn-primary">Registrarse</button>
  </form>
</div>
{% endblock %}
```

No olvides crear una plantilla base (`base_generic.html`) si aún no la tienes, que será la estructura principal de tu sitio.

#### 4.3 Probar la autenticación

1. **Migrar las tablas necesarias**:

Asegúrate de que se migren las tablas necesarias para la autenticación:

```bash
python manage.py migrate
```

2. **Crear un superusuario**:

Si deseas acceder al panel de administración de Django, puedes crear un superusuario:

```bash
python manage.py createsuperuser
```

3. **Probar el registro e inicio de sesión**:

Ahora puedes iniciar el servidor y acceder a `http://127.0.0.1:8000/accounts/login/` para probar el inicio de sesión y a `http://127.0.0.1:8000/accounts/logout/` para el cierre de sesión. El registro estará disponible en `http://127.0.0.1:8000/registro/`.

### Paso 5: Crear el modelo Tarea con sus vistas

#### 1. Crear la Nueva Aplicación `tareas`

Desde el directorio raíz de tu proyecto (donde está el archivo `manage.py`), ejecuta el siguiente comando para crear una nueva aplicación llamada `tareas`:

```bash
python manage.py startapp tareas
```

#### 2. Registrar la Aplicación `tareas` en `settings.py`

Abre el archivo `settings.py` y añade `tareas` a la lista de `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    # Otras apps predeterminadas...
    'usuarios',  # Aplicación de usuarios
    'tareas',    # Nueva aplicación de tareas
    'django.contrib.auth',
    'django.contrib.messages',
]
```

#### 3. Definir el Modelo `Task` en la Aplicación `tareas`

Abre el archivo `tareas/models.py` y define el modelo `Task`:

```python
from django.db import models
from django.contrib.auth.models import User

class Task(models.Model):
    title = models.CharField(max_length=255)
    description = models.TextField(blank=True, null=True)
    user = models.ForeignKey(User, on_delete=models.CASCADE, related_name='tasks')
    category = models.CharField(max_length=100)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title

    class Meta:
        ordering = ['-created_at']  # Ordena las tareas más recientes primero
```

#### 4. Crear los Formularios en la Aplicación `tareas`

Crea un archivo `forms.py` dentro de la carpeta `tareas` y define el formulario para las tareas:

```bash
touch tareas/forms.py
```

Luego, abre `tareas/forms.py` y añade el siguiente contenido:

```python
from django import forms
from .models import Task

class TaskForm(forms.ModelForm):
    class Meta:
        model = Task
        fields = ['title', 'description', 'category']
        widgets = {
            'title': forms.TextInput(attrs={'class': 'form-control'}),
            'description': forms.Textarea(attrs={'class': 'form-control', 'rows': 3}),
            'category': forms.TextInput(attrs={'class': 'form-control'}),
        }
```

#### 5. Crear las Vistas en la Aplicación `tareas`

Abre el archivo `tareas/views.py` y define las vistas para el CRUD de tareas:

```python
from django.urls import reverse_lazy
from django.views.generic import ListView, CreateView, UpdateView, DeleteView
from django.contrib.auth.mixins import LoginRequiredMixin
from .models import Task
from .forms import TaskForm

class TaskListView(LoginRequiredMixin, ListView):
    model = Task
    template_name = 'tareas/task_list.html'
    context_object_name = 'tasks'

    def get_queryset(self):
        return Task.objects.filter(user=self.request.user)

class TaskCreateView(LoginRequiredMixin, CreateView):
    model = Task
    form_class = TaskForm
    template_name = 'tareas/task_form.html'
    success_url = reverse_lazy('task-list')

    def form_valid(self, form):
        form.instance.user = self.request.user
        return super().form_valid(form)

class TaskUpdateView(LoginRequiredMixin, UpdateView):
    model = Task
    form_class = TaskForm
    template_name = 'tareas/task_form.html'
    success_url = reverse_lazy('task-list')

class TaskDeleteView(LoginRequiredMixin, DeleteView):
    model = Task
    template_name = 'tareas/task_confirm_delete.html'
    success_url = reverse_lazy('task-list')
```

#### 6. Configurar las URLs para la Aplicación `tareas`

Crea un archivo `urls.py` dentro de la carpeta `tareas` y define las rutas para las vistas de tareas:

```bash
touch tareas/urls.py
```

Luego, abre `tareas/urls.py` y añade el siguiente contenido:

```python
from django.urls import path
from .views import TaskListView, TaskCreateView, TaskUpdateView, TaskDeleteView

urlpatterns = [
    path('', TaskListView.as_view(), name='task-list'),
    path('nueva/', TaskCreateView.as_view(), name='task-create'),
    path('<int:pk>/editar/', TaskUpdateView.as_view(), name='task-update'),
    path('<int:pk>/eliminar/', TaskDeleteView.as_view(), name='task-delete'),
]
```

Ahora, ajusta las URLs del proyecto principal para incluir las rutas de la aplicación `tareas`.

Abre `gestor_tareas/urls.py` y modifica el archivo para incluir las URLs de `tareas`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('django.contrib.auth.urls')),  # URLs de autenticación
    path('registro/', include('usuarios.urls')),             # URL de registro
    path('tareas/', include('tareas.urls')),                # URLs de tareas
    path('', include('tareas.urls')),                        # Ruta raíz muestra las tareas
]
```

> **Nota:** Al incluir `tareas.urls` también en la ruta raíz (`''`), la página principal de tu sitio será la lista de tareas. Si prefieres que la página raíz muestre algo diferente, ajusta las rutas según tus necesidades.

#### 7. Crear las Plantillas para la Aplicación `tareas`

##### a. Plantilla `task_list.html`

Crea la plantilla `task_list.html` en `tareas/templates/tareas/`:

```bash
mkdir -p tareas/templates/tareas
touch tareas/templates/tareas/task_list.html
```

Luego, abre `tareas/templates/tareas/task_list.html` y añade el siguiente contenido:

```html
{% extends 'base_generic.html' %} {% block content %}
<h2>Mis Tareas</h2>
<a href="{% url 'task-create' %}" class="btn btn-primary mb-3">Nueva Tarea</a>
<ul class="list-group">
  {% for task in tasks %}
  <li class="list-group-item">
    <h5>{{ task.title }}</h5>
    <p>{{ task.description }}</p>
    <small>Categoría: {{ task.category }}</small>
    <div class="mt-2">
      <a href="{% url 'task-update' task.pk %}" class="btn btn-sm btn-warning"
        >Editar</a
      >
      <a href="{% url 'task-delete' task.pk %}" class="btn btn-sm btn-danger"
        >Eliminar</a
      >
    </div>
  </li>
  {% empty %}
  <li class="list-group-item">No tienes tareas.</li>
  {% endfor %}
</ul>
{% endblock %}
```

##### b. Plantilla `task_form.html`

Crea la plantilla `task_form.html` en `tareas/templates/tareas/`:

```bash
touch tareas/templates/tareas/task_form.html
```

Luego, abre `tareas/templates/tareas/task_form.html` y añade el siguiente contenido:

```html
{% extends 'base_generic.html' %} {% block content %}
<h2>{% if form.instance.pk %}Editar Tarea{% else %}Nueva Tarea{% endif %}</h2>
<form method="post">
  {% csrf_token %} {{ form.as_p }}
  <button type="submit" class="btn btn-primary">Guardar</button>
  <a href="{% url 'task-list' %}" class="btn btn-secondary">Cancelar</a>
</form>
{% endblock %}
```

##### c. Plantilla `task_confirm_delete.html`

Crea la plantilla `task_confirm_delete.html` en `tareas/templates/tareas/`:

```bash
touch tareas/templates/tareas/task_confirm_delete.html
```

Luego, abre `tareas/templates/tareas/task_confirm_delete.html` y añade el siguiente contenido:

```html
{% extends 'base_generic.html' %} {% block content %}
<h2>¿Estás seguro de que deseas eliminar esta tarea?</h2>
<form method="post">
  {% csrf_token %}
  <button type="submit" class="btn btn-danger">Eliminar</button>
  <a href="{% url 'task-list' %}" class="btn btn-secondary">Cancelar</a>
</form>
{% endblock %}
```

#### 8. Actualizar la Plantilla Base (`base_generic.html`)

Asegúrate de que la plantilla base (`base_generic.html`) tenga enlaces adecuados para navegar entre las páginas de inicio de sesión, registro y tareas. Si ya la creaste anteriormente, revisa que incluya los enlaces necesarios.

Si necesitas crearla, sigue estos pasos:

1. **Crear la carpeta de plantillas a nivel de proyecto**:

   Desde el directorio raíz del proyecto, crea una carpeta `templates` si no la tienes:

   ```bash
   mkdir -p templates
   touch templates/base_generic.html
   ```

2. **Agregar el siguiente contenido a `base_generic.html`**:

   ```html
   <!DOCTYPE html>
   <html lang="es">
     <head>
       <meta charset="UTF-8" />
       <meta name="viewport" content="width=device-width, initial-scale=1.0" />
       <title>{% block title %}Gestor de Tareas{% endblock %}</title>
       <link
         href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/css/bootstrap.min.css"
         rel="stylesheet"
       />
     </head>
     <body>
       <nav class="navbar navbar-expand-lg navbar-light bg-light">
         <div class="container-fluid">
           <a class="navbar-brand" href="{% url 'task-list' %}"
             >Gestor de Tareas</a
           >
           <button
             class="navbar-toggler"
             type="button"
             data-bs-toggle="collapse"
             data-bs-target="#navbarNav"
             aria-controls="navbarNav"
             aria-expanded="false"
             aria-label="Toggle navigation"
           >
             <span class="navbar-toggler-icon"></span>
           </button>
           <div class="collapse navbar-collapse" id="navbarNav">
             <ul class="navbar-nav ms-auto">
               {% if user.is_authenticated %}
               <li class="nav-item">
                 <a class="nav-link" href="#">Hola, {{ user.username }}</a>
               </li>
               <li class="nav-item">
                 <a class="nav-link" href="{% url 'logout' %}">Cerrar Sesión</a>
               </li>
               {% else %}
               <li class="nav-item">
                 <a class="nav-link" href="{% url 'login' %}">Iniciar Sesión</a>
               </li>
               <li class="nav-item">
                 <a class="nav-link" href="{% url 'registro' %}">Registrarse</a>
               </li>
               {% endif %}
             </ul>
           </div>
         </div>
       </nav>
       <div class="container mt-4">
         {% if messages %} {% for message in messages %}
         <div
           class="alert alert-{{ message.tags }} alert-dismissible fade show"
           role="alert"
         >
           {{ message }}
           <button
             type="button"
             class="btn-close"
             data-bs-dismiss="alert"
             aria-label="Close"
           ></button>
         </div>
         {% endfor %} {% endif %} {% block content %} {% endblock %}
       </div>
       <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/js/bootstrap.bundle.min.js"></script>
     </body>
   </html>
   ```

   - **Navegación Dinámica:** Muestra enlaces de "Cerrar Sesión" y el nombre del usuario si está autenticado; de lo contrario, muestra "Iniciar Sesión" y "Registrarse".
   - **Mensajes Flash:** Incluye la posibilidad de mostrar mensajes de éxito, error, etc., usando el sistema de mensajes de Django.
   - **Bootstrap:** Incluye Bootstrap para estilos y componentes responsivos.

#### 9. Realizar las Migraciones Necesarias

Después de definir el modelo `Task` en la aplicación `tareas`, debes realizar las migraciones para crear las tablas correspondientes en la base de datos.

```bash
python manage.py makemigrations tareas
python manage.py migrate
```

#### 10. Crear un Superusuario (Opcional)

Si aún no lo has hecho, puedes crear un superusuario para acceder al panel de administración de Django y gestionar tus tareas directamente desde allí.

```bash
python manage.py createsuperuser
```

Sigue las instrucciones en la terminal para establecer el nombre de usuario, correo electrónico y contraseña.

#### 11. Iniciar el Servidor y Probar las Funcionalidades

Inicia el servidor de desarrollo de Django:

```bash
python manage.py runserver
```

Abre tu navegador y accede a `http://127.0.0.1:8000/` para ver la lista de tareas. Deberías poder:

- **Registrarte:** Ve a `http://127.0.0.1:8000/registro/` para crear una nueva cuenta.
- **Iniciar Sesión:** Ve a `http://127.0.0.1:8000/accounts/login/` para iniciar sesión.
- **Crear Tareas:** Una vez autenticado, ve a la página principal (`/`) y usa el botón "Nueva Tarea" para crear una nueva tarea.
- **Editar y Eliminar Tareas:** En la lista de tareas, usa los botones "Editar" y "Eliminar" para gestionar tus tareas.
