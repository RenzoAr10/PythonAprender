Crear una carpeta
Abrir el simbolo del sistema
Cd Hasta acceder a la carpeta
> pip install virtualenv
> virtualenv venv    
> .\venv\Scripts\activate    

Abrir VisualStudioCode   
Abrir la carpeta   
En la terminal de la carpeta:    
> pip install django     
> django-admin startproject "nombre_proyecto" # "" no ira     

Django tiene la particularidad que las funcionalides del sw se divide en "apps"   
> python manage.py startapp "nombre_app" # "" no ira      

Ahora desde el proyecto principal se configurara las apps   

**Estructura de una app**    
*views.py* :     
Definir que enviar para que pueda ver el usuario    
*Migrations*:    
Se ira llenando de acuerdo a la base de datos    
*admin.py*:    
Añadir las aplicaciones, administrar     
*apps.py*:    
Configurar las aplicaciones    
*models.py*:    
Crear clases q luego se convertira en tablas de SQL    

```python
# En views.py de una APP
from django.http import HttpResponse
def hello (request):
    return HttpResponse ("<h1>Hello World</h1>")

# En urls.py del proyecto principal
from myapp.views import hello, about
urlpatterns = [
    path("admin/", admin.site.urls),
    path('',hello)
]

# RECORDAR SIEMPRE GUARDAR (Control + S)
# En la terminal python manage.py runserver
```
Cada aplicacion va guardar sus propias URLS   
## Modelos y Bases de datos con SQLite   
Para actualizar en DB Browser for SQlite   
> python manage.py makemigrations
> python manage.py migrate

#### En models.py de Myapp:  
```python
# Para crear una tabla 
class Project (models.Model):
    name = models.CharField(max_length=200)
```
#### En Mysite, settings.py :
```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    # Aqui se colocare para conectar el proyecto principal con myapp
    "myapp"
]
```
Luego se debe guardar y colocar:
>python manage.py makemigrations myapp
>python manage.py migrate myapp 

#### Siguiendo settings.py:
En la seccion de Databases, se puede conectar con otra BD

## Django shell## Django Shell
> Python manage.py shell
>from myapp.models import Project, Task !Son las tablas creadas
>p= Project (name = "aplicación móvil")
>p.save()
>Project.objects.all() !Muestra una lista con todos losobjetos
>Project.objects.get(id=1) !Muestra el objeto correspondiente
>exit()

Para ejecutar tareas
> Python manage.py shell
>from myapp.models import Project, Task
>p = Project.objects.get(id=1)
>p.task_set.all() ! las tareas asociadas a ese proyecto
>p.task_set.create(title = "Descargar IDE",description="Descargar contenido e informacion sobre IDE")
>p.task_set.get(id=1) ! Mostrara el objeto de task con el id=1 del proyecto p
>Project.objects.filter(name__starswith="aplicacion") !que comienze con "aplica"

##Params
En views.py
```python
def hello (request, username):
    return HttpResponse ("<h1>Hello %s</h1>" % username)
```
Y en urls.py   
    path('hello/<str:username>', views.hello),


## Params y Models

En views.py
```python
from .models import Project, Task

def tasks(request, id):
    # ts = Task.objects.get(id=id)
    ts =get_object_or_404(Task,id=id)
    return HttpResponse("Tasks %s" % ts.title)

def projects(request):
    ps = Project.objects.values() # es un query(conjunto de elementos)
    p = list(ps)
    return JsonResponse(p, safe=False)
```
Y en urls.py
    path('projects', views.projects),
    path('tasks/<int:id>', views.tasks),

## Django Admin
>python manage.py createsuperuser
>hp
>renzoaldavereyes@gmail.com
>password1234

##Render
Anteriormente solo devolvía string, ahora se renderizara HTML 
##Templates pass data 

##jinja loops
para recorrer elementos como el for o while
{% for pr in projects %}

<h2>{{pr.name}}</h2>

{% endfor %}

## jinja conditionals
{% if variable == condición %}
## template inheritance
una base.html se crea para que otras puedan heredarlo

## Formularios
crear en myapp un archivo forms.py

