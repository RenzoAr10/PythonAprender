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

```
