Want to setup a simple CMS with FEINCMS but can't find a proper tutorial?

The following explanations try to make you do it step by step.
If something is missing, and you know what, please send a pull request.
If something is missing, and you don't know what, please send a mail to dev.tresontani@gmail.com

If you are bored with the tutorial and just want that to be working, go to the section I PULLED.

PREREQUISITES:
--------------

To start this installation, you will already need:
 sqlite3, python

FEINCMS installation:
---------------------

[Optional] you may want to create a special virtualenv for this, then do:

	mkvirtualenv feincms

[Optional] you may also want versionning for that, then do:

	git init


- Install required packages

	pip install PIL django django-mptt feincms

- Start your django project

	django-admin startproject feincms

- in feincms directory, create your app

	python manage.py startapp cms

- Add this in settings.py INSTALLED APPS 

    ...
    'feincms',
    'feincms.module.page',
    'feincms.module.medialibrary',
    'cms',
    ...

- and uncomment

	'django.contrib.admin' in settings.py

- Edit also your TEMPLATE_DIRS settings and add a template folder

	TEMPLATE_DIRS = ( "<your-path>/templates",)

- and change your database settings with

	DATABASES = {
		'default': {
			'ENGINE': 'django.db.backends.sqlite3', # Add 'postgresql_psycopg2', 'postgresql', 'mysql', 'sqlite3' or 'oracle'.
			'NAME': 'test_feincms',                      # Or path to database file if using sqlite3.


- In urls.py, uncomment:

	from django.contrib import admin
	admin.autodiscover()

	and

	url(r'^admin/', include(admin.site.urls)),

- and add

	 urlpatterns += patterns('',
    	       url(r'', include('feincms.urls')),
         )


- Get feincms media by doing
	
	run python manage.py collectstatic

- in cms/models.py, copy:

	# Create your models here.
	from django.utils.translation import ugettext_lazy as _

	from feincms.module.page.models import Page
	from feincms.content.richtext.models import RichTextContent

	Page.register_templates({
	    'title': _('Standard template'),
	    'path': 'base.html',
	    'regions': (
		('main', _('Main content area')),
		('sidebar', _('Sidebar'), 'inherited'),
		),
	    })

	Page.create_content_type(RichTextContent)


- run

	python manage.py syncdb 

- and create an admin user
- add a base.html template containing:

	<div id="content">
	    {% block content %}
	    {% for content in feincms_page.content.main %}
		{{ content.render }}
	    {% endfor %}
	    {% endblock %}
	</div>

	<div id="sidebar">
	    {% block sidebar %}
	    {% for content in feincms_page.content.sidebar %}
		{{ content.render }}
	    {% endfor %}
	    {% endblock %}
	</div>

- go to the admin page and log in as admin
- go to sites and change the example.com with localhost:8000
- go to Pages and click add
- Add a richtext
- click view on site

THAT'S IT
