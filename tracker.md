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
