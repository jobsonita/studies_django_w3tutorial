[Read this file in English](../../README.md)

# studies_django_w3tutorial

Tutorial de Django do W3Schools: https://www.w3schools.com/django/

> Observação: todos os comandos abaixo presumem uma máquina Unix. Para Windows ou Mac, os comandos podem diferir.

> Observação: `django-admin` e `manage.py` são intercambiáveis em projetos com apenas uma aplicação, mas `django-admin` é mais recomendado ao trabalhar com múltiplas aplicações no mesmo projeto.

## Estrutura do Django

[MVT (Model View Template)](https://www.w3schools.com/django/django_intro.php)

### Modelo

Dados provenientes do banco de dados. Entregues como um Mapeamento Objeto-Relacional (Object Relational Mapping - ORM).

> `models.py`  
> Exemplo: [`members/models.py`](https://github.com/jobsonita/studies_django_w3tutorial/blob/20d6cb7aee5691cf0adaa54df35a1717edde9177/members/models.py)

### View (Visualização, Exibição)

Gerenciador da requisição. Devolve o conteúdo e o layout relevantes.

> `views.py`  
> Exemplo: [`members/views.py`](https://github.com/jobsonita/studies_django_w3tutorial/blob/c6e79a9ab13056a7ba9a340090b472d8509a6e63/members/views.py)

### Template (Modelo de Layout)

Layout da página web com a lógica de exibição dos dados.

> `**/templates/**/*.html`  
> Exemplo: [`members/templates/myfirst.html`](https://github.com/jobsonita/studies_django_w3tutorial/blob/c6e79a9ab13056a7ba9a340090b472d8509a6e63/members/templates/myfirst.html)

### URLs

Decide qual view irá atender a requisição a uma URL específica.

> `urls.py`  
> Exemplos:  
>> [`my_tennis_club/urls.py`](https://github.com/jobsonita/studies_django_w3tutorial/blob/4f1b08fd7e77fa8295ae42eec9fd77089a6d3417/my_tennis_club/urls.py)  
>> [`members/urls.py`](https://github.com/jobsonita/studies_django_w3tutorial/blob/4f1b08fd7e77fa8295ae42eec9fd77089a6d3417/members/urls.py)

### Migrações

Define como o banco de dados é construído para corresponder aos nossos modelos.

> `**/migrations/**/*.py`  
> Exemplos:  
>> [`members/migrations/0001_initial.py`](members/migrations/0001_initial.py)  
>> [`members/migrations/0002_member_joined_date_member_phone.py`](members/migrations/0002_member_joined_date_member_phone.py)

### Settings (Configurações)

Um arquivo importante onde configuramos nosso projeto.

> `settings.py`  
> Exemplo: [`my_tennis_club/settings.py`](https://github.com/jobsonita/studies_django_w3tutorial/blob/c6e79a9ab13056a7ba9a340090b472d8509a6e63/my_tennis_club/settings.py)

## Checagem da instalação do Python

Execute os seguintes comandos no terminal para checar se Python e PIP estão instalados:

```bash
python --version
```

```bash
pip --version
```

## Virtual Environment (Ambiente Virtual)

É recomendado ter um ambiente virtual separado para cada projeto do Django.

Para criar um ambiente virtual, usamos o seguinte comando:

> python -m venv <nome_do_diretorio>

`<nome_do_diretorio>` pode ser qualquer nome, neste projeto usaremos o nome `venv`:

```bash
python -m venv venv
```

Sua estrutura será semelhante a:

> venv
> - bin  
> - include
> - lib
> - pyvenv.cfg

Após a criação do ambiente virtual, nós o ativamos:

```bash
source venv/bin/activate
```

> **Importante**: é necessário ativar o ambiente virtual sempre que reabrirmos o terminal para trabalhar em um projeto.

Após ativá-lo, o terminal ficará semelhante a:

> (venv) @user ... $ 

## Instalando o Django

Instale o Django com o seguinte comando:

```bash (venv)
python -m pip install Django
```

Verifique a instalação do Django executando o seguinte comando:

```bash (venv)
django-admin --version
```

## Criando um Projeto do Django

Para criar um projeto do Django, execute o comando:

> django-admin startproject <nome_do_projeto>

`<nome_do_projeto>` pode ser qualquer nome (que respeite as regras de nomenclatura de variáveis do Django), neste projeto usaremos o nome `my_tennis_club`

Além disso, passamos um argumento adicional, o caminho atual, "`.`", para forçar o Django a criar o projeto na raiz do repositório, uma vez que não teremos outros projetos nele (facilita a execução de comandos com `manage.py`, pois assim não precisamos especificar a aplicação ou navegar entre diretórios com frequência).

```bash (venv)
django-admin startproject my_tennis_club .
```

Esse comando cria o arquivo `manage.py` na raiz do repositório, e também o diretório de projeto `my_tennis_club` com a seguinte estrutura:

> my_tennis_club
> - \_\_init\_\_.py
> - asgi.py
> - settings.py
> - urls.py
> - wsgi.py

## Criando uma Aplicação

Um app é uma aplicação web. Neste projeto, será um sistema de gerenciamento de membros de um clube de tênis.

Nós criamos novas aplicações através do seguinte:

> ./manage.py startapp <nome_da_aplicacao>

Neste projeto, o `<nome_da_aplicacao>` será `members`:

```bash (venv)
./manage.py startapp members
```

Esse comando cria o diretório `members` com a seguinte estrutura:

> members
> - migrations
>   - \_\_init\_\_.py
> - \_\_init\_\_.py
> - admin.py
> - apps.py
> - models.py
> - tests.py
> - views.py

## Criando migrações para modelos novos ou modificados

Sempre que modificamos o arquivo `models.py` de uma aplicação para incluir ou alterar modelos, precisamos criar migrações para informar ao banco de dados como entrar em conformidade com nossas alterações. Fazemos isso através do seguinte comando:

> ./manage.py makemigrations <nome_da_aplicacao> --name <nome_da_migracao>

Neste projeto, modificamos um modelo na aplicação `members`, e aceitamos o nome gerado automaticamente (`initial` para a primeira migração), então podemos ignorar o argumento `--name` e executar apenas:

```bash (venv)
./manage.py makemigrations members
```

Finalmente, aplicamos nossas migrações através do comando:

```bash (venv)
./manage.py migrate
```

## Terminal interativo

Ao executar o comando abaixo, podemos interagir com o projeto pelo terminal interativo:

```bash (venv)
./manage.py shell
```

Podemos importar os modelos e interagir como a seguir:

```python shell
from members.models import Member
Member.objects.all().values()
# <QuerySet []>
member = Member(firstname='Emil', lastname='Refsnes')
member.save()
Member.objects.all().values()
# <QuerySet [{'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes'}]>
```

Assumindo que um diretório `fixtures` exista na aplicação `members` (crie o diretório se ele não existir), podemos fazer o backup dos dados em um arquivo através do seguinte comando:

```bash (venv)
./manage.py dumpdata members -o members/fixtures/members.json
```

Dessa forma, podemos restaurar o banco de dados em outra máquina executando o seguinte comando:

```bash (venv)
./manage.py loaddata members/fixtures/members.json --app members
```

Podemos também excluir um registro do banco de dados manualmente pelo terminal interativo:

```python shell
from members.models import Member
member = Member.objects.all()[5]
member.delete()
```

## QuerySet (Conjunto de Consulta)

### \<Model\>.objects.all()

Obtém todos os registros de um modelo como um Conjunto de Consulta (QuerySet).

### \<Model\>.objects.all().values()

Obtém os objetos como dicionários do Python.

### \<Model\>.objects.values_list(fields)

Obtém os objetos restringindo os campos aos campos indicados.

```python
def testing(request):
    mymembers = Member.objects.values_list('firstname', 'lastname')
    ...
```

### \<Model\>.objects.filter(rules).values()

Obtém apenas os objetos que correspondem às regras especificadas.

```python
def testing(request):
    mymembers = Member.objects.filter(firstname='Emil', lastname='Refsnes').values()
    ...
```

### União de consultas

```python
def testing(request):
    mymembers = Member.objects.filter(firstname='Emil').values() | Member.objects.filter(firstname='Tobias').values()
    ...
```

Alternativa usando `django.db.models.Q`:

```python
from django.db.models import Q

def testing(request):
    mymembers = Member.objects.filter(Q(firstname='Emil') | Q(firstname='Tobias')).values()
    ...
```

### Field Lookups (Pesquisas de Campo)

```python
def testing(request):
    mymembers = Member.objects.filter(firstname__startswith='L').values()
    ...
```

Outras palavras-chave de pesquisa:

https://www.w3schools.com/django/django_queryset_filter.php

### order_by (Ordenação)

```python
def testing(request):
    # ordem ascendente
    mymembers = Member.objects.all().order_by('firstname').values()
    ...
    # ordem descendente
    mymembers = Member.objects.all().order_by('-firstname').values()
    ...
    # ordenação por múltiplos campos
    mymembers = Member.objects.all().order_by('firstname', '-lastname').values()
```

## Compartilhando este projeto com outros desenvolvedores

Quando desejar, execute o comando abaixo para gerar uma lista de requisitos a serem instalados:

```bash (venv)
pip freeze > requirements.txt
```

Em outra máquina, restaure o projeto executando os seguintes comandos:

```bash (venv)
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Se o projeto possuir migrações, certifique-se de executá-las para preparar o banco de dados:

```bash (venv)
./manage.py migrate
```

Caso precise preencher o banco de dados com registros deste tutorial, use o seguinte comando:

```bash (venv)
./manage.py loaddata members/fixtures/members.json --app members
```

## Executando o projeto

Para executar o projeto, use o seguinte comando:

```bash (venv)
./manage.py runserver
```

## Padrões de Url do Django

### URL Params (Parâmetros da URL)

Para captar os parâmetros da url, usamos a seguinte notação:

> `<type:name>`

onde `type` pode ser [`int`, `str`, `uuid`, `slug` ou `path`](https://docs.djangoproject.com/en/4.2/topics/http/urls/#path-converters), ou outros casadores (matchers) mais complexos, e `name` é o nome da variável que será passada para a view.

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

## Linguagem de Template do Django

### Contexto

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

Exemplo: [members/views.py](https://github.com/jobsonita/studies_django_w3tutorial/blob/df7c73961e899aff3aec3418ed8da6c880063a7c/members/views.py)

### exibição de dados

```django
<p>{{ variable_to_be_displayed }}</p>
```

Exemplo: [members/templates/all_members.html](https://github.com/jobsonita/studies_django_w3tutorial/blob/df7c73961e899aff3aec3418ed8da6c880063a7c/members/templates/all_members.html)

### with variable ("com a variável")

```django
{% with firstname="Tobias" %}
  <h1>Olá {{ firstname }}, como vai você?</h1>
{% endwith %}
```

> **Importante**: não pode haver espaços entre a variável, o símbolo de igual e o valor.

### for loop e empty (laço for e vazio)

```django
{% for item in a_list_of_items %}
  <p id="paragraph{{ item.id }}">{{ item.field }}</p>
{% empty %}
  <p>Nenhum item encontrado.</p>
{% endfor %}
```

Exemplo: [members/templates/all_members.html](https://github.com/jobsonita/studies_django_w3tutorial/blob/df7c73961e899aff3aec3418ed8da6c880063a7c/members/templates/all_members.html)

Variáveis do laço for:

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
  <h1>Olá</h1>
{% elif greeting == 2 %}
  <h1>Bem vindo</h1>
{% else %}
  <h1>Até mais</h1>
{% endif %}
```

### block e extends (bloco e estende)

Podemos ter um layout pai com a estrutura geral da página, e layouts filhos que estendem esse layout e preenchem os blocos predefinidos pelo pai:

```django
<!-- template.html (layout pai) -->
<!DOCTYPE html>
<html>
  <head>
    <title>{% block title %}{% endblock %}</title>
  </head>
  <body>
    {% block content %}{% endblock %}
  </body>
</html>

<!-- page.html (layout filho) -->
{% extends 'template.html' %}

{% block title %}Título da página{% endblock %}

{% block content %}
  <h1>Título do conteúdo</h1>
  <p>Conteúdo da página</p>
{% endblock %}
```

### include (inclui)

```django
<!-- nav.html -->
<div>HOME | {{ eu }} | SOBRE | FÓRUM | {{ patrocinador }}</div>

<!-- footer.html -->
<p>Você chegou ao rodapé desta página.</p>

<!-- page.html -->
<!DOCTYPE html>
<html>
<body>
  {% include "nav.html" with eu="TOBIAS" patrocinador="W3SCHOOLS" %}
  
  <h1>Olá</h1>
  <p>Esta página contém layouts de navegação e de rodapé.</p>

  {% include 'footer.html' %}
</body>
</html>
```

### Comentários

```django
{% comment %}
  <h1>Este elemento será ignorado</h1>
{% endcomment %}
{% comment "com uma descrição com o intuito de documentar algo" %}
  <h2>Este elemento também será ignorado</h2>
{% endcomment %}
<h1>Olá{# Todos #}</h1>
```

## Páginas Especiais

### 404 - Not Found (Não Encontrado)

Contanto que haja um layout chamado `404.html` dentro da aplicação, ele será usado sempre que erros 404 ocorrerem.

## Administração do Django

### Superusuário

Para criar um superusuário e obter acesso à interface de administração do Django (caminho `/admin`), execute:

```bash (venv)
./manage.py createsuperuser
```

### Registro do modelo

Para acessarmos nossos modelos pela interface de administração do Django, precisamos registrá-los em `admin.py`:

```python
from django.contrib import admin
from .models import Member

admin.site.register(Member)
```

Também podemos configurar quais campos são exibidos na view de listagem:

```python
from django.contrib import admin
from .models import Member

class MemberAdmin(admin.ModelAdmin):
  list_display = ("firstname", "lastname", "joined_date",)

admin.site.register(Member, MemberAdmin)
```

## Problemas comuns

### Pylance e Venv

Ao abrir um arquivo Python, talvez a extensão Pylance reclame sobre não ser possível resolver as importações. Isto se deve ao VSCode usar a instalação global do Python ao invés da presente no ambiente virtual `venv`. Isto deve ser corrigido individualmente em cada máquina, uma vez que a estrutura do venv varia com o sistema operacional.

Consulte https://code.visualstudio.com/docs/python/environments#_working-with-python-interpreters para resolver o problema.

### Django Admin, Codespaces e CSRF

Ao acessar a interface de administração do Django, se ocorrer um erro de CSRF, considere a adição da linha abaixo ao arquivo `settings.py`:

```python
if 'CODESPACE_NAME' in os.environ:
    CSRF_TRUSTED_ORIGINS = [f'https://{os.getenv("CODESPACE_NAME")}-8000.{os.getenv("GITHUB_CODESPACES_PORT_FORWARDING_DOMAIN")}']
```
