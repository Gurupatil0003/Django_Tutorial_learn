
```python
pip install django
```

```python
django-admin startproject myproject
cd myproject
python manage.py startapp myapp

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
