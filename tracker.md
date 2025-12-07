```python
python -m django startproject myproject

python -m django startapp tracker

```

### views.py

```python
from django.shortcuts import render, redirect
from .models import Expense

def home(request):
    if request.method == "POST":
        title = request.POST.get("title")
        amount = request.POST.get("amount")
        Expense.objects.create(title=title, amount=amount)
        return redirect('home')

    expenses = Expense.objects.all()
    total = sum(e.amount for e in expenses)

    return render(request, 'home.html', {
        'expenses': expenses,
        'total': total
    })


```

### tracker/urls.py

```python
from django.urls import path
from .views import home

urlpatterns = [
    path('', home, name='home'),
]


```

### project urls.py

```python

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('tracker.urls')),
]


```

### setting.py

```python
STATIC_URL = '/static/'

STATIC_URL = '/static/'

STATICFILES_DIRS = [
    BASE_DIR / "static"
]


STATICFILES_DIRS = [BASE_DIR / "static"]

```

### tracker/static/tracker/style.css

```python
body {
    font-family: Arial, sans-serif;
    background-color: #f4f6f9;
    margin: 0;
    padding: 0;
}

.container {
    width: 400px;
    margin: 50px auto;
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}

h1 {
    text-align: center;
    color: #333;
}

form {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

input {
    padding: 10px;
    font-size: 16px;
}

button {
    background-color: #4CAF50;
    color: white;
    padding: 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #45a049;
}

ul {
    list-style: none;
    padding: 0;
}

li {
    background: #f2f2f2;
    margin: 5px 0;
    padding: 10px;
    border-radius: 5px;
}

.total {
    text-align: center;
    font-size: 20px;
    font-weight: bold;
    margin: 15px 0;
}


```

```python
document.addEventListener("DOMContentLoaded", function () {
    const form = document.querySelector("form");
    const title = document.querySelector("input[name='title']");
    const amount = document.querySelector("input[name='amount']");

    form.addEventListener("submit", function (e) {
        if (title.value.trim() === "" || amount.value.trim() === "") {
            alert("Please fill all fields!");
            e.preventDefault();
        }
    });
});


```
## models.py
```python
from django.db import models

class Expense(models.Model):
    title = models.CharField(max_length=100)
    amount = models.IntegerField()

    def __str__(self):
        return self.title

```

```python
<!DOCTYPE html>
<html>
<head>
    <title>Expense Tracker</title>
    {% load static %}
    <link rel="stylesheet" href="{% static 'tracker/style.css' %}">
</head>
<body>
    <div class="container">
        <h1>💰 Expense Tracker</h1>

        <form method="POST" id="expenseForm">
            {% csrf_token %}
            <input type="text" name="title" id="title" placeholder="Expense name" required>
            <input type="number" name="amount" id="amount" placeholder="Amount" required>
            <button type="submit">Add Expense</button>
        </form>

        <h2>Total: ₹<span id="total">{{ total }}</span></h2>

        <ul>
            {% for expense in expenses %}
                <li>{{ expense.title }} - ₹{{ expense.amount }}</li>
            {% endfor %}
        </ul>
    </div>

    <script src="{% static 'tracker/script.js' %}"></script>
</body>
</html>

```

```python
<!DOCTYPE html>
<html>
<head>
    <title>Expense Tracker</title>
</head>
<body>
    <h1>Expense Tracker</h1>

    <form method="POST">
        {% csrf_token %}
        <input type="text" name="title" placeholder="Expense name" required>
        <input type="number" name="amount" placeholder="Amount" required>
        <button type="submit">Add Expense</button>
    </form>

    <h2>Total: ₹{{ total }}</h2>

    <ul>
        {% for expense in expenses %}
            <li>{{ expense.title }} - ₹{{ expense.amount }}</li>
        {% endfor %}
    </ul>
</body>
</html>


```

## categories.html
```python
{% load static %}
<!DOCTYPE html>
<html>
<head>
    <title>Categories</title>
    <link rel="stylesheet" href="{% static 'tracker/style.css' %}">
</head>
<body>

<div class="container">

<h1>📂 Category Report</h1>

<a href="/" class="back-link">← Back to Home</a>

<ul>
    {% for item in data %}
        <li>
            <span>{{ item.category }}</span>
            <span>₹{{ item.total }}</span>
        </li>
    {% endfor %}
</ul>

</div>
</body>
</html>


```

## home.html

```python
{% load static %}
<!DOCTYPE html>
<html>
<head>
    <title>Expense Tracker</title>
    <link rel="stylesheet" href="{% static 'tracker/style.css' %}">
</head>
<body>

<div class="container">

<h1>💰 Expense Tracker</h1>

<nav>
    <a href="/stats/">Statistics</a>
    <a href="/categories/">Categories</a>
</nav>

<form method="POST">
    {% csrf_token %}
    <input type="text" name="title" placeholder="Expense name" required>
    <input type="number" name="amount" placeholder="Amount" required>

    <select name="category">
        <option>Food</option>
        <option>Transport</option>
        <option>Shopping</option>
        <option>Bills</option>
        <option>Other</option>
    </select>

    <button type="submit">Add Expense</button>
</form>

<div class="total-box">
    Total: ₹{{ total }}
</div>

<ul>
    {% for expense in expenses %}
        <li>
            <span>{{ expense.title }} ({{ expense.category }})</span>
            <span>₹{{ expense.amount }}</span>
        </li>
    {% endfor %}
</ul>

</div>
</body>
</html>

```

## stats.html
```python
{% load static %}
<!DOCTYPE html>
<html>
<head>
    <title>Statistics</title>
    <link rel="stylesheet" href="{% static 'tracker/style.css' %}">
</head>
<body>

<div class="container">

<h1>📊 Statistics</h1>

<a href="/" class="back-link">← Back to Home</a>

<div class="card">
    Total Expenses: ₹{{ total }}
</div>

<div class="card">
    Highest Expense: ₹{{ highest }}
</div>

</div>
</body>
</html>

```
## style.css
```python
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Segoe UI', sans-serif;
}

body {
    background-color: #eef2f7;
    padding: 40px 20px;
    color: #2c3e50;
}

/* Main card */
.container {
    max-width: 720px;
    margin: auto;
    background-color: #ffffff;
    padding: 28px 32px;
    border-radius: 16px;
    box-shadow: 0 15px 40px rgba(0, 0, 0, 0.08);
}

/* Title */
h1 {
    text-align: center;
    font-size: 28px;
    margin-bottom: 25px;
    letter-spacing: 0.5px;
}

/* Navigation */
nav {
    display: flex;
    justify-content: center;
    gap: 20px;
    margin-bottom: 30px;
}

nav a {
    text-decoration: none;
    background-color: #f1f4f9;
    padding: 10px 22px;
    border-radius: 999px;
    color: #3b82f6;
    font-weight: 600;
    transition: all 0.2s ease;
}

nav a:hover {
    background-color: #e0e7ff;
    transform: translateY(-1px);
}

/* Form */
form {
    display: flex;
    flex-direction: column;
    gap: 14px;
    margin-bottom: 20px;
}

input, select {
    padding: 13px 15px;
    border-radius: 12px;
    border: 1px solid #dde3ec;
    background-color: #fafbfc;
    font-size: 15px;
    transition: border-color 0.2s ease;
}

input:focus, select:focus {
    outline: none;
    border-color: #3b82f6;
}

/* Button */
button {
    margin-top: 10px;
    padding: 13px;
    border-radius: 12px;
    border: none;
    background-color: #3b82f6;
    color: #fff;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

button:hover {
    transform: translateY(-1px);
    box-shadow: 0 10px 20px rgba(59, 130, 246, 0.3);
}

/* Total Box */
.total-box {
    text-align: center;
    background-color: #f1f5f9;
    padding: 15px;
    margin: 25px 0 20px;
    border-radius: 14px;
    font-weight: 600;
    font-size: 17px;
}

/* Expense list */
ul {
    list-style: none;
}

ul li {
    background-color: #f8fafc;
    border: 1px solid #e5e7eb;
    border-radius: 12px;
    padding: 14px 16px;
    margin-bottom: 10px;
    font-size: 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

/* Back button */
.back-link {
    display: inline-block;
    margin-bottom: 20px;
    padding: 8px 18px;
    background-color: #f1f5f9;
    color: #3b82f6;
    border-radius: 999px;
    text-decoration: none;
    font-weight: 600;
}

.back-link:hover {
    background-color: #e0e7ff;
}

/* Cards */
.card {
    background-color: #f8fafc;
    border: 1px solid #e5e7eb;
    border-radius: 16px;
    padding: 20px;
    margin-top: 15px;
    font-size: 17px;
    text-align: center;
    font-weight: 600;
}

```

## models.py
```python
from django.db import models

class Expense(models.Model):
    title = models.CharField(max_length=100)
    amount = models.IntegerField()
    category = models.CharField(max_length=50, default="General")

    def __str__(self):
        return self.title

```

## urls.py
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
    path('stats/', views.stats, name='stats'),
    path('categories/', views.categories, name='categories'),
]


```

## views.py

```python
from django.shortcuts import render, redirect
from .models import Expense
from django.db.models import Sum, Max

def home(request):
    if request.method == "POST":
        title = request.POST.get("title")
        amount = request.POST.get("amount")
        category = request.POST.get("category")
        Expense.objects.create(title=title, amount=amount, category=category)
        return redirect('home')

    expenses = Expense.objects.all()
    total = sum(e.amount for e in expenses)

    return render(request, 'home.html', {
        'expenses': expenses,
        'total': total
    })


def stats(request):
    total = Expense.objects.aggregate(Sum('amount'))['amount__sum'] or 0
    highest = Expense.objects.aggregate(Max('amount'))['amount__max'] or 0

    return render(request, 'stats.html', {
        'total': total,
        'highest': highest
    })


def categories(request):
    data = Expense.objects.values('category').annotate(total=Sum('amount'))

    return render(request, 'categories.html', {
        'data': data
    })


```
