Crear una carpeta
Abrir el simbolo del sistema
Cd Hasta acceder a la carpeta
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

## Django shell

