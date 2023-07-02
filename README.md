# studies_django_w3tutorial

W3Schools Django Tutorial: https://www.w3schools.com/django/

> Note: all commmands bellow assume a Unix machine. For Windows or Mac, the commands might differ.

> Note: `django-admin` and `manage.py` can be used interchangeably in projects with a single app, but `django-admin` is preferred when working with multiple apps in the same project.

## Django structure:

[MVT (Model View Template)](https://www.w3schools.com/django/django_intro.php)

### Model

Data from the database. Delivered as an Object Relational Mapping (ORM).

> `models.py`

### View

Request handler. Returns the relevant template and content.

> `views.py`

### Template

Layout of the web page with logic on how to display the data.

> `**/templates/**/*.html`

### URLs

Decides which view will handle a request to a specific URL.

> `urls.py`

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

## Creating migrations for new or modified models:

Whenever we change the `models.py` file of an app to include new models or change old ones, we must create migrations to tell the database to comply to those changes. We do that by running the following command:

> ./manage.py makemigrations <app_name> --name <migration_name>

In our case, we're changing a model in the `members` app, and we're fine with the auto generated name (`initial` for the first migration, `auto` for the later ones), so we can ignore the `--name` argument and just run:

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

## Common problems

When opening a Python file, you might notice Pylance complains about not being able to resolve imports. This is due to VSCode automatically picking the system's Python installation instead of the `venv` one. This must be fixed on each machine individually, since the venv structure varies with the OS.

Refer to https://code.visualstudio.com/docs/python/environments#_working-with-python-interpreters to fix that problem.
