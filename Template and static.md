Create a Django project:
First, create a Django project if you haven't already. You can do this by running the following command in your terminal:

~~~python
django-admin startproject myproject
~~~
Create a Django app:
Navigate to your project directory and create a Django app using the following commands
~~~python
cd myproject
python manage.py startapp myapp


~~~
Create HTML file:
Inside the myapp directory, create an HTML file. Let's name it index.html. You can do this manually or by using a text editor. Here's a basic HTML structure you can start with:

html
~~~python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Django App</title>
    <link rel="stylesheet" type="text/css" href="{% static 'css/style.css' %}">
</head>
<body>
    <h1>Hello, Django!</h1>
</body>
</html>

~~~
Configure settings:
In your Django project's settings file (settings.py), make sure you have the following configurations:

~~~python

STATIC_URL = '/static/'

# Add the following line if STATICFILES_DIRS is not already defined
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'myapp/static'),
]


~~~
URL configuration at the project level:
Open the urls.py file in your project directory (myproject). Define URL patterns for your Django app (myapp) within the project's urls.py file. For example:

~~~python

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),  # Include app-specific URLs
]
~~~
Create CSS file:
Inside the myapp directory, create a folder named static and inside that folder create another folder named css. Then, create a CSS file named style.css inside the css folder. Here's an example CSS code:

~~~python
body {
    background-color: #f0f0f0;
    font-family: Arial, sans-serif;
}

h1 {
    color: #333;
}

~~~

URL configuration:
In your Django app's urls.py file, create a URL pattern to map to your HTML page. For example:

~~~python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]

~~~

URL configuration at the app level:
Keep the URL configuration within your app (myapp) unchanged as shown in the previous response:

~~~python

from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
~~~
View function:
In your Django app's views.py file, create a view function to render the HTML page. For example:

~~~python

from django.shortcuts import render

def index(request):
    return render(request, 'myapp/index.html')

~~~
Run the server:
Finally, run the Django development server:
~~~python
python manage.py runserver
~~~
