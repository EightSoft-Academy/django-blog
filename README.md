# BOOK: Django 3 By Examples (03.07.2020)
## CHAPTER 1: Building a Blog Application

### Steps for creating a project and apps:
1. Create a virtual env and start a project
``` bash
python -m venv my_env
.\my_env\Scripts\activate
deactivate
pip install "Django==3.0.*"
django-admin startproject my_project
```
2. Run a development server and create an app
``` bash
python manage.py runserver
python manage.py startapp my_app
```
3. designe models in models.py
4. add/activate the application in the list of INSTALLED_APPS->
'my_app.apps.My_AppConfig'
5. generate (create & apply) migrations->
``` bash
python manage.py makemigrations my_app 
python manage.py sqlmigrate my_app 0001 <-returns the SQL or generates a table without executing it.
python manage.py migrate
```
6. create an administration site for your models->
``` bash
python manage.py createsuperuser
```
7. TIPS-2 admin.py-> 
``` python
from .models import My_Model 
admin.site.register(My_Model)
```
8. TIPS-5-6-7 (views.py->urls.py->.html)-> building views, urls, templates and static 
9. python manage.py collectstatic
10. adding pagination (p.s. different in ClassBasedView)-> pages[34-36]
11. TIPS-8 views.py-> Class based views

---
For keeping all apps in one folder, add in settings.py->
PROJECT_ROOT = os.path.dirname(__file__)
sys.path.insert(0, os.path.join(PROJECT_ROOT, 'apps'))
---

#### TIPS-1: models.py If you edit the models.py file in order to add, remove, or change the fields of existing models, or if you add new models, you will have to create a new migration using the 'makemigrations' command. The migration will allow Django to keep track of model changes. Then, you will have to apply it with the 'migrate' command to keep the database in sync with your models.

---
#### TIPS-2: admin.py Customizing the way that models are displayed:
``` python
  from django.contrib import admin
  from .models import Post
  @admin.register(Post)
  class PostAdmin(admin.ModelAdmin):
     list_display = ('title', 'slug', 'author', 'publish', 'status')
  <------The power of Django------>
  @admin.register(Post)
  class PostAdmin(admin.ModelAdmin):
     list_display = ('title', 'slug', 'author', 'publish', 'status')
     list_filter = ('status', 'created', 'publish', 'author')
     search_fields = ('title', 'body')
     prepopulated_fields = {'slug': ('title',)}
     raw_id_fields = ('author',)
     date_hierarchy = 'publish'
     ordering = ('status', 'publish')
```
---
#### TIPS-3: QuerySets and managers="'objects' e.i. Post.objects.all()", (Django ORM is based in them, they retrieve objects from db, we can apply filters to them), ORM(Object relational model)-- register databases in DATABASES, u can use multiple db at the same time->
* CREATING OBJECTS (in case of blog app)->
  python manage.py shell
  >>> from django.contrib.auth.models import User
  >>> from blog.models import Post
  >>> user = User.objects.get(username='admin')
  >>> post = Post(title='Another post',
  ... slug='another-post',
  ... body='Post body.',
  ... author=user)
  >>> post.save()

We get QuerySet objects using managers, each Django model has at least one model, and the default manager is 'objects'->
  My_Model.objects.get()
  My_Model.objects.all()
  My_Model.objects.filter(publish__year=2020, author__username='admin')
  My_Model.objects.exclude(title__startswith='Why')
  My_Model.objects.order_by('-title')
---
#### TIPS-4: models.py Creating model managers(in case of blog app)-> 1. add to default or 2. create a new ->
``` python
  class PublishedManager(models.Manager):
      def get_queryset(self):
          return super(PublishedManager, self).get_queryset()\
                                              .filter(status='published')

  class Post:
  # ... ...
  objects = models.Manager() # The default manager.
  published = PublishedManager() # Our custom manager.
  ```
---
#### TIPS-5: views.py->
``` python
  from django.shortcuts import render
  from .models import Post
  def post_list(request):
      posts = Post.published.all()
      return render(request, 'blog/post/list.html', {'posts': posts})
 ```
---
#### TIPS-6: my_app/urls.py-> DO NOT FORGET TO INCLUDE APP IN ROOT urls.py ALSO ('blog.apps.BlogConfig',)!
``` python
  from django.urls import path
  from . import views
  app_name = 'blog'
  urlpatterns = [
     # post views
     path('', views.post_list, name='post_list'),
     path('<int:year>/<int:month>/<int:day>/<slug:post>/',
     views.post_detail,
     name='post_detail'),
  ]
  ```
---
#### TIPS-7: templates-> create a folder for each app inside templates folder->
 1. templates/
       blog/
           base.html
           post/
	      list.html
	      detail.html
       app2/... ...
 2. {% tag %}, {{ variable }}, {{ variable | filter}}
 3. {% load static %} after loading it we can use {% static "css/base.css" %}
 4. in post/list.html we must write-> {% extends "blog/base.html" %}
    {% block content %} & {% endblock %}
 5. settings.py->
    STATIC_URL = '/static/'
    STATIC_DIR = os.path.join(BASE_DIR, 'static')
    STATICFILES_DIRS = [STATIC_DIR]
    and in TEMPLATES add-> 'DIRS': [os.path.join(PROJECT_ROOT, 'templates')],
---
#### TIPS-8: views.py-> When we use ClassBasedViews we must change->
  1. the path(".as_view()") in my_app/urls.py
  2. change into {% include "blog/pagination.html" with page=page_obj %}

## CHAPTER 2: Enhancing Your Blog with Advanced Features
In this chapter, we will cover the following topics:
• Sending emails with Django
• TIPS-9 Creating forms and handling them in views
• Creating forms from models
• Integrating third-party applications
• Building complex QuerySets
 
#### TIPS-9 sharing posts via e-mail:
1. 
    - create a form for users to fill email and name;
    - create a view that handles post and sends email;
    - add a URL in urls.py for the new view;
    - create a template to display the form;
2. adding comments to a post
3. tagging posts
4. recommending similar posts

TIPS-9: my_app/forms.py
  Django has two base classes to build forms: 
    * Form and ModelForm
    * from django import forms
  
