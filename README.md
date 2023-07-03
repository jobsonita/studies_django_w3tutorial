# studies_django_w3tutorial

W3Schools Django Tutorial: https://www.w3schools.com/django/

> Note: all commmands bellow assume a Unix machine. For Windows or Mac, the commands might differ.

> Note: `django-admin` and `manage.py` can be used interchangeably in projects with a single app, but `django-admin` is preferred when working with multiple apps in the same project.

## Django structure

[MVT (Model View Template)](https://www.w3schools.com/django/django_intro.php)

### Model

Data from the database. Delivered as an Object Relational Mapping (ORM).

> `models.py`  
> Example: [`members/models.py`](https://github.com/jobsonita/studies_django_w3tutorial/blob/20d6cb7aee5691cf0adaa54df35a1717edde9177/members/models.py)

### View

Request handler. Returns the relevant template and content.

> `views.py`  
> Example: [`members/views.py`](https://github.com/jobsonita/studies_django_w3tutorial/blob/c6e79a9ab13056a7ba9a340090b472d8509a6e63/members/views.py)

### Template

Layout of the web page with logic on how to display the data.

> `**/templates/**/*.html`  
> Example: [`members/templates/myfirst.html`](https://github.com/jobsonita/studies_django_w3tutorial/blob/c6e79a9ab13056a7ba9a340090b472d8509a6e63/members/templates/myfirst.html)

### URLs

Decides which view will handle a request to a specific URL.

> `urls.py`  
> Examples:  
>> [`my_tennis_club/urls.py`](https://github.com/jobsonita/studies_django_w3tutorial/blob/4f1b08fd7e77fa8295ae42eec9fd77089a6d3417/my_tennis_club/urls.py)  
>> [`members/urls.py`](https://github.com/jobsonita/studies_django_w3tutorial/blob/4f1b08fd7e77fa8295ae42eec9fd77089a6d3417/members/urls.py)

### Migrations

Define how the database is built to match our models.

> `**/migrations/**/*.py`  
> Examples:  
>> [`members/migrations/0001_initial.py`](members/migrations/0001_initial.py)  
>> [`members/migrations/0002_member_joined_date_member_phone.py`](members/migrations/0002_member_joined_date_member_phone.py)

### Settings

An important file where we configure our project.

> `settings.py`  
> Example: [`my_tennis_club/settings.py`](https://github.com/jobsonita/studies_django_w3tutorial/blob/c6e79a9ab13056a7ba9a340090b472d8509a6e63/my_tennis_club/settings.py)

## Checking Python installation

Run the following commands on the terminal to see whether Python and PIP are installed:

```bash
python --version
```

```bash
pip --version
```

## Virtual Environment

It is recommended to have a virtual environment for each Django project.

To create a virtual environment, we use the following command:

> python -m venv <folder_name>

`<folder_name>` can be any name, in this project we'll use the name `venv`:

```bash
python -m venv venv
```

Its structure will look like the following:

> venv
> - bin  
> - include
> - lib
> - pyvenv.cfg

Once the virtual environment is created, we activate it:

```bash
source venv/bin/activate
```

> **Important**: you must activate the virtual environment every time you open the terminal to work on your project.

Once activated, the terminal will look like the following:

> (venv) @user ... $ 

## Installing Django

Install Django with the following command:

```bash (venv)
python -m pip install Django
```

Check Django installation by running the command:

```bash (venv)
django-admin --version
```

## Creating a Django Project

To create a Django project, run the command:

> django-admin startproject <project_name>

`<project_name>` can be any name, in this project we'll use the name `my_tennis_club`

Also, we'll pass an additional argument, the current path, "`.`", to force Django to create the project at the root of the repository, since we won't have any other projects in it (it makes it easier to run `manage.py` commands, by not having to navigate inside an extra folder).

```bash (venv)
django-admin startproject my_tennis_club .
```

It will create a `manage.py` file on the root of the repository and a `my_tennis_club` project folder with the following structure:

> my_tennis_club
> - \_\_init\_\_.py
> - asgi.py
> - settings.py
> - urls.py
> - wsgi.py

## Creating an App

An app is a web application. In this project, it'll be a tennis club member management system.

We create new apps by running the command below:

> ./manage.py startapp <app_name>

In our case, the `<app_name>` will be `members`:

```bash (venv)
./manage.py startapp members
```

It will create a `members` folder with the following structure:

> members
> - migrations
>   - \_\_init\_\_.py
> - \_\_init\_\_.py
> - admin.py
> - apps.py
> - models.py
> - tests.py
> - views.py

## Creating migrations for new or modified models

Whenever we change the `models.py` file of an app to include new models or change old ones, we must create migrations to tell the database to comply to those changes. We do that by running the following command:

> ./manage.py makemigrations <app_name> --name <migration_name>

In our case, we're changing a model in the `members` app, and we're fine with the auto generated name (`initial` for the first migration), so we can ignore the `--name` argument and just run:

```bash (venv)
./manage.py makemigrations members
```

Finally, we apply our migrations through:

```bash (venv)
./manage.py migrate
```

## Interactive shell

We are able to interact with our project through the interactive shell by running:

```bash (venv)
./manage.py shell
```

We can import our models and interact with them like so:

```python shell
from members.models import Member
Member.objects.all().values()
# <QuerySet []>
member = Member(firstname='Emil', lastname='Refsnes')
member.save()
Member.objects.all().values()
# <QuerySet [{'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes'}]>
```

Assuming a `fixtures` folder exists in `members` app (create it if it doesn't), we can back up the data to a file with the following command:

```bash (venv)
./manage.py dumpdata members -o members/fixtures/members.json
```

This way, we can restore the database on a new machine by running:

```bash (venv)
./manage.py loaddata members/fixtures/members.json --app members
```

We can delete a record through the interactive shell like so:

```python shell
from members.models import Member
member = Member.objects.all()[5]
member.delete()
```

## QuerySet

### \<Model\>.objects.all()

Retrieves all records of a model as a QuerySet.

### \<Model\>.objects.all().values()

Retrieves the objects as Python dictionaries.

### \<Model\>.objects.values_list(fields)

Retrieves the objects with only the specified columns (fields) filled.

```python
def testing(request):
    mymembers = Member.objects.values_list('firstname', 'lastname')
    ...
```

### \<Model\>.objects.filter(rules).values()

Retrieves only the objects which fulfill the specified rules.

```python
def testing(request):
    mymembers = Member.objects.filter(firstname='Emil', lastname='Refsnes').values()
    ...
```

### Union of queries

```python
def testing(request):
    mymembers = Member.objects.filter(firstname='Emil').values() | Member.objects.filter(firstname='Tobias').values()
    ...
```

Alternative using `django.db.models.Q`:

```python
from django.db.models import Q

def testing(request):
    mymembers = Member.objects.filter(Q(firstname='Emil') | Q(firstname='Tobias')).values()
    ...
```

### Field Lookups

```python
def testing(request):
    mymembers = Member.objects.filter(firstname__startswith='L').values()
    ...
```

Other lookup keywords:

https://www.w3schools.com/django/django_queryset_filter.php

## Sharing current project with other developers

At any moment, run the following command to generate a list of requirements to be installed:

```bash (venv)
pip freeze > requirements.txt
```

On another machine, restore the project by running the following commands:

```bash (venv)
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

If the project has any migrations, make sure to run them to prepare the database:

```bash (venv)
./manage.py migrate
```

If you need to fill the database with data from this tutorial, use the following command:

```bash (venv)
./manage.py loaddata members/fixtures/members.json --app members
```

## Running the project

To run the project, use the following command:

```bash (venv)
./manage.py runserver
```

## Django Url Patterns

### URL Params

To catch a parameter from the url, we use the notation:

> `<type:name>`

where `type` could be [`int`, `str`, `uuid`, `slug` and `path`](https://docs.djangoproject.com/en/4.2/topics/http/urls/#path-converters), or other more advanced matchers, and `name` is the name of the variable that will be passed on to the view.

```python
# urls.py
from django.urls import path

from . import views

urlpatterns = [
    path('app/users/<int:id>', views.user_details, name='user_details'),
]

# views.py
from django.http import HttpResponse
from django.template import loader
from .models import User


def user_details(request, id):
    user = User.objects.get(id=id)
    template = loader.get_template('user_details.html')
    context = {
        'user': user,
    }
    return HttpResponse(template.render(context, request))
```

## Django Template Tags

### Context

```python
from django.http import HttpResponse
from django.template import loader


def members(request):
    template = loader.get_template('all_members.html')
    context = {
        "variable_to_be_displayed": "Hello World!",
        "a_list_of_items": [
            {"id": 1, "field": "value 1"},
            {"id": 2, "field": "value 2"},
        ],
    }
    return HttpResponse(template.render(context, request))
```

Example: [members/views.py](https://github.com/jobsonita/studies_django_w3tutorial/blob/df7c73961e899aff3aec3418ed8da6c880063a7c/members/views.py)

### data display

```django
<p>{{ variable_to_be_displayed }}</p>
```

Example: [members/templates/all_members.html](https://github.com/jobsonita/studies_django_w3tutorial/blob/df7c73961e899aff3aec3418ed8da6c880063a7c/members/templates/all_members.html)

### with variable

```django
{% with firstname="Tobias" %}
  <h1>Hello {{ firstname }}, how are you?</h1>
{% endwith %}
```

> **Important**: there must not be any spaces between the variable, the assignment and the value.

### for loop and empty

```django
{% for item in a_list_of_items %}
  <p id="paragraph{{ item.id }}">{{ item.field }}</p>
{% empty %}
  <p>No items.</p>
{% endfor %}
```

Example: [members/templates/all_members.html](https://github.com/jobsonita/studies_django_w3tutorial/blob/df7c73961e899aff3aec3418ed8da6c880063a7c/members/templates/all_members.html)

Loop variables:

> - forloop.counter
> - forloop.counter0
> - forloop.first
> - forloop.last
> - forloop.parentloop
> - forloop.revcounter
> - forloop.revcounter0

```django
{% for item in a_list_of_items reversed %}
  <li
    {% if forloop.first %}
      style="background-color:lightblue;"
    {% endif %}
  >{{ forloop.couter }}</li>
{% endfor %}
```

### if elif else

```django
{% if greeting == 1 %}
  <h1>Hello</h1>
{% elif greeting == 2 %}
  <h1>Welcome</h1>
{% else %}
  <h1>Goodbye</h1>
{% endif %} 
```

### block and extends

We can have a parent template with the general structure of the page, and children templates that extend that template and fill in the blocks set up by the parent:

```django
<!-- template.html (parent template) -->
<!DOCTYPE html>
<html>
  <head>
    <title>{% block title %}{% endblock %}</title>
  </head>
  <body>
    {% block content %}{% endblock %}
  </body>
</html>

<!-- page.html (child template) -->
{% extends 'template.html' %}

{% block title %}Page title{% endblock %}

{% block content %}
  <h1>Content title</h1>
  <p>Content of the page</p>
{% endblock %}
```

### include

```django
<!-- nav.html -->
<div>HOME | {{ me }} | ABOUT | FORUM | {{ sponsor }}</div>

<!-- footer.html -->
<p>You have reached the bottom of this page.</p>

<!-- page.html -->
<!DOCTYPE html>
<html>
<body>
  {% include "nav.html" with me="TOBIAS" sponsor="W3SCHOOLS" %}
  
  <h1>Hello</h1>
  <p>This page contains a nav and a footer templates.</p>

  {% include 'footer.html' %}
</body>
</html>
```

### Comments

```django
{% comment %}
  <h1>This element will be ignored</h1>
{% endcomment %}
{% comment "with a description for documentation" %}
  <h2>This element will also be ignored</h2>
{% endcomment %}
<h1>Welcome{# Everyone #}</h1>
```

## Special Pages

### 404 - Not Found

As long as there is a template file named `404.html` inside an app, it will be used whenever a 404 error occurs.

## Django Admin

### Superuser

To create the superuser in order to access Django Admin interface (`/admin` endpoint), we run:

```bash (venv)
./manage.py createsuperuser
```

### Model registration

In order to access our models in Django Admin interface, we need to register them at `admin.py`:

```python
from django.contrib import admin
from .models import Member

admin.site.register(Member)
```

We can also configure which fields are displayed in the list view:

```python
from django.contrib import admin
from .models import Member

class MemberAdmin(admin.ModelAdmin):
  list_display = ("firstname", "lastname", "joined_date",)

admin.site.register(Member, MemberAdmin)
```

## Common problems

### Pylance and Venv

When opening a Python file, you might notice Pylance complains about not being able to resolve imports. This is due to VSCode automatically picking the system's Python installation instead of the `venv` one. This must be fixed on each machine individually, since the venv structure varies with the OS.

Refer to https://code.visualstudio.com/docs/python/environments#_working-with-python-interpreters to fix that problem.

### Django Admin, Codespaces and CSRF

When accessing the Django Admin interface, if it gives an error about CSRF, consider adding this line to the `settings.py` file:

```python
if 'CODESPACE_NAME' in os.environ:
    CSRF_TRUSTED_ORIGINS = [f'https://{os.getenv("CODESPACE_NAME")}-8000.{os.getenv("GITHUB_CODESPACES_PORT_FORWARDING_DOMAIN")}']
```
