
```python
pip install virtualenv  
virtualenv venv   
venv\Scripts\activate

```


```python
pip install django
```

```python
django-admin startproject myproject
cd myproject
django-admin startapp myapp

```

2. Configure settings.py
In myproject/settings.py, add myapp to INSTALLED_APPS:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp',  # Add this line
]

```

3. Create a View in myapp/views.py
```python

from django.shortcuts import render

def home(request):
    users = [
        {"name": "Alice", "age": 25},
        {"name": "Bob", "age": 30},
        {"name": "Charlie", "age": 22},
    ]
    return render(request, "index.html", {"title": "User List", "users": users})
```

4. Set Up URLs
Modify myproject/urls.py:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),  # Include app URLs
]


```

Create myapp/urls.py:
```python

from django.urls import path
from .views import home

urlpatterns = [
    path('', home, name='home'),
]

```


5. Create a Template
Create a folder templates/ inside myapp/, then add index.html.

File: myapp/templates/index.html

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ title }}</title>
</head>
<body>
    <h1>{{ title }}</h1>
    <ul>
        {% for user in users %}
            <li>{{ user.name }} (Age: {{ user.age }})</li>
        {% endfor %}
    </ul>
</body>
</html>

```

2. Configure TEMPLATES in settings.py
Open myproject/settings.py and update the TEMPLATES setting:
```python
import os

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'myapp/templates')],  # Add this line
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

6. Run the Django Server
Apply migrations and start the server:
```python
python manage.py migrate
python manage.py runserver
```




### lab 2
- Steps to Build a Debugging Web Application in Django
- Create a Django Project: Start by creating a Django project.
- If you haven't done that yet, run the following commands:

```python
pip install virtualenv

python -m venv myenv

myenv\Scripts\activate


pip install django
```
```python

django-admin startproject debug_app
cd debug_app

```

```python

django-admin startapp main
```
- Configure settings.py:
- Open debug_app/settings.py and ensure the DEBUG mode is turned on. 
- By default, Django sets DEBUG = True for development purposes, but you can verify it here.
 ```python
DEBUG = True  # Make sure this is set to True during development

```
- Set Up URL Patterns: Open debug_app/urls.py and add the main app to the URL configurations:
``` python

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('main.urls')),  # Include the URLs from the 'main' app
]
```

- Create main/urls.py:
-  Inside the main app directory, create a urls.py file to define the URLs for the views that will trigger errors and exceptions.
  main/urls.py:

from django.urls import path
```python
from . import views
from django.urls import path, include


urlpatterns = [
    path('', views.index, name='index'),
    path('error/', views.error_view, name='error_view'),  # Add an endpoint that causes an error
]

urlpatterns = [
    path('', views.index, name='index'),
    path('error/', views.error_view, name='error_view'),  # Add an endpoint that causes an error
]
```

- Create Views to Trigger Errors:
- Open main/views.py and create two views:

- One view (index) that works correctly.
- One view (error_view) that intentionally triggers an error.
  main/views.py:

 ``` python
from django.shortcuts import render
from django.http import HttpResponse

def index(request):
   return HttpResponse("Welcome to the Debugging App!")

def error_view(request):
   try:
       result = 1 / 0  # This will raise a ZeroDivisionError
   except ZeroDivisionError as e:
       # Handle the error, log it, or display a friendly error message
       return HttpResponse("Oops! Something went wrong. Please try again later.")

```
- Run the Development Server: With Django’s DEBUG = True, Django will display detailed error pages for any unhandled exceptions. Start the development server:
``` python
  python manage.py runserver
```




# Lab 3 Ajax

```python 

pip install virtualenv

python -m venv myenv

myenv\Scripts\activate


pip install django

```

# Setups

```python
django-admin startproject project1

cd project1
```

# then next  create ur view.py file and add this code i mentioned here

```python

from django.shortcuts import render
from django.http import JsonResponse
def home(request):
    return render(request, 'index.html')

def python_funct(request):
      #do something with the data passed

      fname=request.GET.get('fname',None)
      lname=request.GET.get('lname',None)
      fullname= fname+' '+lname
      #fullname= 33
      response = {
        'fullname': fullname
         }
      return JsonResponse(response)
```
# create a folder templates/html file. Add this code here below
```python

<html>
<head>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script>
function fullName() {
//var fname= document.getElementById("fname").value;
//var lname= document.getElementById("lname").value;
//var fullname={%url "python_funct"%}

//ajax call to python function
//$("#datadiv").find("select, textarea, input").serialize()
$.ajax({data:$("#datadiv").find("input").serialize(),
url: "{% url 'python_funct' %}",
type: "GET",
success: successFunc,
error: errorFunc
});

function successFunc(response) {
        //alert('ok');
        document.getElementById("p1").innerHTML = response.fullname;
    }
function errorFunc() {
        alert('error');
    }

//fullname=2;
//var fullname=2;

}
</script>
</head>

<body>

<div align=center id="datadiv">
<p>Hello!</p>
First Name :<input type=text name=fname id=fname><br>
Last Name  :<input type=text name=lname id=lname><br>
<button onclick="fullName()">What's full name?</button>
<p id="p1">t</p>
<div>

</body>
</html>
```

# After that url creation look into urls.py file and add this file

```python

"""project1 URL Configuration

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/3.2/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path
from project1 import views
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.home, name='home'),
    path('pfunct', views.python_funct, name='python_funct'),
]
```
