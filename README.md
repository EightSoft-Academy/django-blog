# Building a Blog Application

## CHECKPOINT 1: 
Building a blog application:
- Installing Django
- Creating and configuring a Django project
- Creating a Django application
- Designing models and generating model migrations
- Creating an administration site for your models
- Working with QuerySets and managers
- Building views, templates, and URLs
- Adding pagination to list views
- Using Django's class-based views

1. Create a virtual env and start a project
``` bash
python -m venv my_env
.\my_env\Scripts\activate
deactivate
pip install Django
django-admin startproject mysite
```
2. Run a development server and create an app
``` bash
python manage.py runserver
python manage.py startapp blog
```
3. Design models 
4. Add/activate the application from INSTALLED_APPS->
``my_app.apps.My_AppConfig``
5. Generate (create & apply) migrations->
``` bash
python manage.py makemigrations blog 
python manage.py sqlmigrate blog 0001 <- returns the SQL or generates a table without executing it.
python manage.py migrate
```
6. Create an administration site for your models->
``python manage.py createsuperuser``
7. admin.py
``` python
from .models import MyModel 
admin.site.register(MyModel)
```
8. views.py-urls.py-.html || building views, urls, templates and static ``python manage.py collectstatic``

9. For keeping all apps in one folder, add in settings.py ->
``PROJECT_ROOT = os.path.dirname(__file__)
sys.path.insert(0, os.path.join(PROJECT_ROOT, 'apps'))``

- TIPS-1: models.py - If you edit the models.py file in order to add, remove, or change the fields of existing models, or if you add new models, you will have to create a new migration using the 'makemigrations' command. The migration will allow Django to keep track of model changes. Then, you will have to apply it with the 'migrate' command to keep the database in sync with your models.

- TIPS-2: QuerySets and managers="'objects' e.i. Post.objects.all()", (Django ORM is based in them, they retrieve objects from db, we can apply filters to them), ORM(Object relational model)-- register databases in DATABASES, u can use multiple db at the same time.

## CHECKPOINT 2
Enhancing Your Blog with Advanced Features:
- Sending emails with Django
- Creating forms and handling them in views
- Creating forms from models
- Integrating third-party applications
- Building complex QuerySets
 
1. sharing posts via e-mail: 
    - create a form for users to fill email and name;
    - create a view that handles post and sends email;
    - add a URL in urls.py for the new view;
    - create a template to display the form;
2. adding comments to a post
3. tagging posts
4. recommending similar posts

## CHECKPOINT 3
Extending your blog application:
- Creating custom template tags 
- Adding a sitemap and post feed (root urls)
- Implementing full text search with PostgreSQL (SearchVector, SearchQuery, SearchRank, Searching with trigram similarity)

- TIPS-1: simple_tag & inclusion_tag & filter

- TIPS-2: ``(Post.objects.filter(body__contains='framework')`` without Postgres
``Post.objects.filter(body__search='django')`` with Postgres

- TIPS-3: (searching by multiple field = title and body, stemming(music, musician) and ranking results)

- TIPS-4: Other full-text search engines
You may want to use a full-text search engine other than from PostgreSQL. If you
want to use Solr or Elasticsearch, you can integrate them into your Django project
using Haystack. Haystack is a Django application that works as an abstraction
layer for multiple search engines. It offers a simple search API that is very similar
to Django QuerySets. You can find more information about Haystack at https://
django-haystack.readthedocs.io/en/master/.
