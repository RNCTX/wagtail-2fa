.. start-no-pypi

.. image:: https://dev.azure.com/lab-digital-opensource/wagtail-2fa/_apis/build/status/labd.wagtail-2fa?branchName=master
    :target: https://dev.azure.com/lab-digital-opensource/wagtail-2fa/_build/latest?definitionId=3&branchName=master

.. image:: http://codecov.io/github/labd/wagtail-2fa/coverage.svg?branch=master
    :target: http://codecov.io/github/labd/wagtail-2fa?branch=master

.. image:: https://img.shields.io/pypi/v/wagtail-2fa.svg
    :target: https://pypi.python.org/pypi/wagtail-2fa/

.. image:: https://readthedocs.org/projects/wagtail-2fa/badge/?version=stable
    :target: https://wagtail-2fa.readthedocs.io/en/stable/?badge=stable

.. image:: https://img.shields.io/github/stars/labd/wagtail-2fa.svg?style=social&logo=github
    :target: https://github.com/labd/wagtail-2fa/stargazers

.. end-no-pypi

===========
wagtail-2fa
===========

This Django app add's two factor authentication to Wagtail. Behind the scenes
it use django-otp_ which supports Time-based One-Time Passwords (TOTP). This
allows you to use various apps like Authy, Google Authenticator, or
1Password.


.. _django-otp: https://django-otp-official.readthedocs.io


Installation
============

.. code-block:: shell

   pip install wagtail-2fa


Then add the following lines to the ``INSTALLED_APPS`` list in your Django
settings:

.. code-block:: python

    INSTALLED_APPS = [
        # ...
        'wagtail_2fa',
        'django_otp',
        'django_otp.plugins.otp_totp',
        # ...
    ]

Migrate your database:

.. code-block:: shell

   python manage.py migrate

Next add the required middleware to the ``MIDDLEWARE``. It should come
after the AuthenticationMiddleware:

.. code-block:: python

    MIDDLEWARE = [
        # .. other middleware
        # 'django.contrib.auth.middleware.AuthenticationMiddleware',

        'wagtail_2fa.middleware.VerifyUserMiddleware',

        # 'wagtail.core.middleware.SiteMiddleware',
        # .. other middleware
    ]


Settings
========

The following settings are available (Set via your Django settings):

    - ``WAGTAIL_2FA_REQUIRED`` (default ``False``): When set to True all
      staff, superuser and other users with access to the Wagtail Admin site
      are forced to login using two factor authentication.
    - ``WAGTAIL_MOUNT_PATH`` (default: ``''``): The uWSGI mount point that
      Wagtail is running at. Ex. ``/wagtail``
    - ``WAGTAIL_2FA_OTP_TOTP_NAME`` (default: ``False``): The issuer name to
      identify which site is which in your authenticator app. If not set and
      ``WAGTAIL_SITE_NAME`` is defined it uses this. sets ``OTP_TOTP_ISSUER``
      under the hood.



Sandbox
=======
First create a new virtualenv with Python 3.6.1 and activate it. Then run
the following commands:

    - make sandbox

You can then visit http://localhost:8000/admin/ and login with the following
credentials:

    - E-mail: superuser@example.com
    - Password: testing
