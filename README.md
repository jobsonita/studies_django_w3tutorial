# studies_django_w3tutorial

W3Schools Django Tutorial: https://www.w3schools.com/django/

Note: all commmands bellow assume a Unix machine. For Windows or Mac, the commands might differ.

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

If the project has any migrations, make sure to run them to restore the database:

```bash (venv)
python manage.py migrate
```
