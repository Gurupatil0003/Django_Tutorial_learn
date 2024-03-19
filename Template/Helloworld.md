# Hello world In Django
- Install Django: If you haven't already installed Django, you can do so using pip, Python's package manager. You can install Django by running pip install django in your command line or terminal.

- Create a Django Project: You can create a new Django project using the django-admin command-line utility. Navigate to the directory where you want to create your project and run the following command:

```python
django-admin startproject myproject
```
- Create a Django App: Inside your Django project, you'll typically create one or more Django apps. Each app serves a specific functionality within your project. Navigate into your project directory and create a new app using the following command:

```python
python manage.py startapp myapp
```
- Define a View: In Django, views are Python functions that take web requests and return web responses. Open the views.py file inside your app (myapp/views.py) and define a simple view like this:

```python
 #myapp/views.py
from django.http import HttpResponse

def hello_world(request):
    return HttpResponse("Hello, World!")
```
- URL Mapping: You need to map your view to a URL so that Django knows which view to execute when a specific URL is requested. Open the urls.py file inside your app (myapp/urls.py) and define a URL pattern like this:

```python
#myapp/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.hello_world, name='hello_world'),
]
```
- Include the App URLs in the Project URLs: Open the urls.py file inside your project (myproject/urls.py) and include the URLs of your app like this:

```python
#myproject/urls.py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),
]
```
- Run the Development Server: Start the Django development server by running the following command in your project directory:

```python
python manage.py runserver
```
- Access the Hello World Page: Open your web browser and go to http://127.0.0.1:8000/ (or http://localhost:8000/). You should see "Hello, World!" displayed on the page.
