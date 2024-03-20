### Create ur project 
```python
django-admin startproject myproject
```
#           Or
```python
python -m django startproject mysite

```
### Create Ur app
```python
django-admin startapp myapp
```

### makemigrations is a management command
```python
python manage.py makemigrations

```


migrate is a management command
```python
python manage.py migrate
```
```python
python manage.py runserver

```
```python
python manage.py createsuperuser
```

## run your server

```python
python manage.py runserver

```
### Use below url /admin with current localhost
```python
 Use thsis url http://127.0.0.1:8000/admin
```


### output
```python

Username (leave blank to use 'lenovo'): Guru
Email address: gurupatil@gmail.com
Password:
Password (again):

Bypass password validation and create user anyway? [y/N]: y        
Superuser created successfully.
```
```python
python manage.py shell

```

```python
from django.contrib.auth.models import User
user = User.objects.create_superuser(username='Gurupatil',
                                 email='gurupatil327@gmai.com',
                                 password='ur')
```
```python
user.is_staff
False
 user.is_active
True
 user.is_superuser
False


```


### makemigrations:

- makemigrations is a management command that is used to create migration files based on the changes you have made to your models (i.e., changes to your database schema).
  When you make changes to your models (e.g., add new models, modify existing fields, etc.), Django detects these changes and generates migration files that represent the changes to be applied to the database schema.
  These migration files are Python files located in the migrations directory of each app in your Django project.
  makemigrations does not actually modify the database schema; it only generates the migration files based on the changes to your models.

### migrate:

- migrate is a management command that is used to apply migrations and synchronize the database schema with the changes defined in the migration files.
 When you run migrate, Django examines the migration files and applies the necessary changes to the database schema to reflect the changes in your models.
 migrate is responsible for creating, updating, or deleting database tables, fields, and indexes based on the migration files.
 It ensures that the database schema is consistent with the state of your models and migrations.
