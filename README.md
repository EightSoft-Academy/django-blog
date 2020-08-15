# Django 3 By Examples
## CHAPTER 1: Building a Blog Application

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
8. views.py-urls.py-.html || building views, urls, templates and static 
9. ``python manage.py collectstatic``

For keeping all apps in one folder, add in settings.py ->
``PROJECT_ROOT = os.path.dirname(__file__)
sys.path.insert(0, os.path.join(PROJECT_ROOT, 'apps'))``


- TIPS-1: models.py - If you edit the models.py file in order to add, remove, or change the fields of existing models, or if you add new models, you will have to create a new migration using the 'makemigrations' command. The migration will allow Django to keep track of model changes. Then, you will have to apply it with the 'migrate' command to keep the database in sync with your models.

- TIPS-2: QuerySets and managers="'objects' e.i. Post.objects.all()", (Django ORM is based in them, they retrieve objects from db, we can apply filters to them), ORM(Object relational model)-- register databases in DATABASES, u can use multiple db at the same time->
* CREATING OBJECTS (in case of blog app)->
  python manage.py shell
  ```
  >>> from django.contrib.auth.models import User
  >>> from blog.models import Post
  >>> user = User.objects.get(username='admin')
  >>> post = Post(title='Another post',
  ... slug='another-post',
  ... body='Post body.',
  ... author=user)
  >>> post.save()
  ```

We get QuerySet objects using managers, each Django model has at least one model, and the default manager is 'objects'->
  My_Model.objects.get()
  My_Model.objects.all()
  My_Model.objects.filter(publish__year=2020, author__username='admin')
  My_Model.objects.exclude(title__startswith='Why')
  My_Model.objects.order_by('-title')

## CHAPTER 2: Enhancing Your Blog with Advanced Features
In this chapter, we will cover the following topics:
- Sending emails with Django
- Creating forms and handling them in views
- Creating forms from models
- Integrating third-party applications
- Building complex QuerySets
 
- sharing posts via e-mail:
1. 
    - create a form for users to fill email and name;
    - create a view that handles post and sends email;
    - add a URL in urls.py for the new view;
    - create a template to display the form;
2. adding comments to a post
3. tagging posts
4. recommending similar posts

