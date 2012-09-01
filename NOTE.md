Offical Installation
=======================
The installation is straightforward and should last just a few minutes. Please execute following steps:

Download the installer from http://pypi.python.org/pypi/django-lfs

    $ tar xzf django-lfs-installer-<version>.tar.gz
    $ cd lfs-installer
    $ python bootstrap.py
    $ bin/buildout -v
    Enter your database settings to lfs_project/settings.py
    $ bin/django syncdb
    $ bin/django lfs_init
    $ bin/django collectstatic
    $ bin/django runserver

Browse to http://localhost:8000


Modify for openshift
=======================

1. Modify buildout.cfg

    [versions]
    django = 1.4.1
    djangorecipe = 1.2.1
    django-lfs = 0.7.6
    mysql-python = 1.2.3

    [django]
    recipe = djangorecipe
    eggs =
        django-lfs
        gunicorn
        mysql-python

2. Modify lfs_project/settings.py

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql', # Add 'postgresql_psycopg2', 'postgresql', 'mysql', 'sqlite3' or 'oracle'.
            'NAME': os.environ['OPENSHIFT_APP_NAME'],                      # Or path to database file if using sqlite3.
            'USER': os.environ['OPENSHIFT_DB_USERNAME'],                      # Not used with sqlite3.
            'PASSWORD': os.environ['OPENSHIFT_DB_PASSWORD'],                  # Not used with sqlite3.
            'HOST': os.environ['OPENSHIFT_DB_HOST'],                      # Set to empty string for localhost. Not used with sqlite3.
            'PORT': os.environ['OPENSHIFT_DB_PORT'],                      # Set to empty string for default. Not used with sqlite3.
        }
    }

    TEMPLATE_LOADERS = (
        'django.template.loaders.filesystem.Loader',
        'django.template.loaders.app_directories.Loader',

    #    'django.template.loaders.filesystem.load_template_source',
    #    'django.template.loaders.app_directories.load_template_source',
    #     'django.template.loaders.eggs.load_template_source',
    )

    TEMPLATE_CONTEXT_PROCESSORS = (
        'django.core.context_processors.debug',
        'django.contrib.auth.context_processors.auth',
    #    'django.core.context_processors.auth',
        'django.core.context_processors.request',
        'django.core.context_processors.media',
        'django.core.context_processors.static',
        'lfs.core.context_processors.main',
    )