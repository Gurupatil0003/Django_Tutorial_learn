
```python
import requests
from django.shortcuts import render
from django.http import JsonResponse

def home(request):
    return render(request, "recipes/home.html")

def search(request):
    query = request.GET.get("q", "")
    url = f"https://www.themealdb.com/api/json/v1/1/search.php?s={query}"
    r = requests.get(url).json()

    return JsonResponse(r)

def recipe_detail(request, id):
    url = f"https://www.themealdb.com/api/json/v1/1/lookup.php?i={id}"
    data = requests.get(url).json()
    return render(request, "recipes/detail.html", {"data": data})



```

## recipe/ursl.py

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home),
    path('search/', views.search),
    path('recipe/<id>/', views.recipe_detail),
]


```

## urls.py

```python
from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('',include('recipes.urls'))
]

```

## detail.html
```python
<!DOCTYPE html>
<html>
<head>
    <title>Recipe Details</title>
    {% load static %}
    <link rel="stylesheet" href="{% static 'recipes/style.css' %}">
</head>
<body>

<div class="wrapper">

    {% if data.meals %}
        {% with meal=data.meals.0 %}
            <h2>{{ meal.strMeal }}</h2>

            <img src="{{ meal.strMealThumb }}" width="300">

            <h3>Instructions</h3>
            <p>{{ meal.strInstructions }}</p>

            <br>
            <a href="/" style="color: #6a5acd; font-weight: bold;">← Back</a>
        {% endwith %}
    {% endif %}

</div>

</body>
</html>


```

## detail.html

```python
<!DOCTYPE html>
<html>
<head>
    <title>Recipe Finder</title>
    {% load static %}
    <link rel="stylesheet" href="{% static 'recipes/style.css' %}">
</head>
<body>

<div class="wrapper">
    <h2>Recipe Finder</h2>

    <input id="search" placeholder="Search recipes..." onkeyup="searchRecipes()">

    <div id="results"></div>
</div>

<script src="{% static 'recipes/main.js' %}"></script>

</body>
</html>

```

## main.js

```python
function searchRecipes() {
    let q = document.getElementById("search").value;

    fetch("/search/?q=" + q)
        .then(r => r.json())
        .then(data => {
            let box = document.getElementById("results");
            box.innerHTML = "";

            if (!data.meals) {
                box.innerHTML = "No results";
                return;
            }

            data.meals.forEach(meal => {
                box.innerHTML += `
                    <div>
                        <a href="/recipe/${meal.idMeal}/">${meal.strMeal}</a>
                    </div>
                `;
            });
        });
}


```

## style.css

```python
/* Global */
body {
    margin: 0;
    background: #f4f6f8;
    font-family: "Segoe UI", Roboto, sans-serif;
    color: #1f1f1f;
}

/* Main wrapper */
.wrapper {
    max-width: 900px;
    margin: 50px auto;
    padding: 0 20px;
}

/* App title */
h2 {
    text-align: center;
    font-size: 34px;
    font-weight: 700;
    background: linear-gradient(45deg, #6a5acd, #845ec2);
    -webkit-background-clip: text;
    color: transparent;
    margin-bottom: 30px;
}

/* Search box */
#search {
    width: 100%;
    padding: 14px 18px;
    font-size: 17px;
    border: 2px solid #dfe3e6;
    border-radius: 14px;
    background: white;
    transition: 0.25s;
    margin-bottom: 25px;
}

#search:focus {
    border-color: #845ec2;
    box-shadow: 0 0 8px rgba(132,94,194,0.2);
}

/* Recipe Grid */
#results {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
    gap: 18px;
}

/* Recipe Card */
.card {
    background: white;
    padding: 18px;
    border-radius: 14px;
    box-shadow: 0 4px 14px rgba(0,0,0,0.06);
    transition: 0.25s;
    cursor: pointer;
}

.card:hover {
    transform: translateY(-4px);
    box-shadow: 0 6px 18px rgba(0,0,0,0.12);
}

.card-title {
    font-size: 18px;
    font-weight: 600;
    margin-top: 10px;
    color: #333;
}

.card img {
    width: 100%;
    border-radius: 12px;
}

/* Detail Page */
.detail-header {
    text-align: center;
}

.detail-header h2 {
    font-size: 32px;
    margin-bottom: 10px;
}

.detail-img {
    width: 320px;
    border-radius: 16px;
    display: block;
    margin: 0 auto 20px;
}

h3 {
    margin-top: 25px;
    font-size: 22px;
    color: #444;
}

p {
    line-height: 1.7;
    font-size: 16px;
}

/* Back link */
.back-link {
    display: inline-block;
    margin-top: 25px;
    padding: 10px 16px;
    background: #845ec2;
    color: white;
    border-radius: 8px;
    text-decoration: none;
    transition: 0.2s;
}

.back-link:hover {
    background: #6a5acd;
}


```
