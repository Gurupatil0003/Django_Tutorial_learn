## Basic 

## Creation
```
django-admin startproject mysite
cd mysite
python manage.py startapp blogs
```

## setup
```python
INSTALLED_APPS = [
    ...
    'pages',
]


```


## blogs/views.py
```python

from django.shortcuts import render

def home(request):
    return render(request, 'pages/home.html')

def about(request):
    return render(request, 'pages/about.html')

def contact(request):
    return render(request, 'pages/contact.html')

```

## blog/urls.py
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home),
    path('about/', views.about),
    path('contact/', views.contact),
]


```

## mysite/urls.py
```python


```


```python
pages/
│
├── templates/
│   └── pages/
│       ├── home.html
│       ├── about.html
│       └── contact.html

```

# home.html
```python
<!DOCTYPE html>
<html>
<head><title>Home</title></head>
<body>
    <h1>Hello from Home Page</h1>
    <a href="/about/">About</a> | <a href="/contact/">Contact</a>
</body>
</html>


```

## about.html
```python
<!DOCTYPE html>
<html>
<head><title>About</title></head>
<body>
    <h1>This is About Page</h1>
    <a href="/">Home</a> | <a href="/contact/">Contact</a>
</body>
</html>

```

## contact.html
```
<!DOCTYPE html>
<html>
<head><title>Contact</title></head>
<body>
    <h1>This is Contact Page</h1>
    <a href="/">Home</a> | <a href="/about/">About</a>
</body>
</html>

```


### 🎯 Folder Setup

```python
django-admin startproject mysite
cd mysite
python manage.py startapp blog
```


## 🔧 1. mysite/settings.py
Add 'blog' in INSTALLED_APPS:

```python
INSTALLED_APPS = [
    ...
    'blog',
]
```


## 🌐 2. mysite/urls.py
```python

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('', include('blog.urls')),
]
```


## 🌍 3. blog/urls.py
```python

from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
    path('add/', views.add_post, name='add_post'),
    path('post/<int:id>/', views.post_detail, name='post_detail'),
]
```

### 🧠 4. blog/views.py (No DB, just memory list)

```python

from django.shortcuts import render, redirect

# In-memory "database"
POSTS = []

def post_list(request):
    return render(request, 'blog/post_list.html', {'posts': POSTS})

def post_detail(request, id):
    post = POSTS[id]
    return render(request, 'blog/post_detail.html', {'post': post})

def add_post(request):
    if request.method == 'POST':
        title = request.POST.get('title')
        content = request.POST.get('content')
        POSTS.append({'title': title, 'content': content})
        return redirect('/')
    return render(request, 'blog/add_post.html')
```

```python
## 🧾 5. HTML Templates
Create folders:


mkdir -p blog/templates/blog
```

## ✅ post_list.html

```python
<!DOCTYPE html>
<html>
<head><title>Blog</title></head>
<body>
    <h1>Blog Posts</h1>
    <a href="/add/">Add New Post</a>
    <ul>
        {% for post in posts %}
            <li><a href="/post/{{ forloop.counter0 }}/">{{ post.title }}</a></li>
        {% endfor %}
    </ul>
</body>
</html>
```

```python
## ✅ add_post.html


<!DOCTYPE html>
<html>
<head><title>Add Post</title></head>
<body>
    <h1>Add New Post</h1>
    <form method="POST">
        {% csrf_token %}
        <input type="text" name="title" placeholder="Title"><br><br>
        <textarea name="content" placeholder="Content"></textarea><br><br>
        <button type="submit">Submit</button>
    </form>
    <a href="/">Back to posts</a>
</body>
</html>
```

```python
## ✅ post_detail.html

<!DOCTYPE html>
<html>
<head><title>{{ post.title }}</title></head>
<body>
    <h1>{{ post.title }}</h1>
    <p>{{ post.content }}</p>
    <a href="/">Back to posts</a>
</body>
</html>
```
### 🚀 Run the App
```python
python manage.py runserver
```


