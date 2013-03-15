BulletShotNews
=================


Bulletin News application for the Django web framework BASED in django-newsletter ( by github.com/dokterbob ) with new specifications, complementations and New Design with list of Bootstrap resources. 
----------------------------------------------------

What is it?
===========
Django app for managing multiple mass-mailing lists with both plaintext as
well as HTML templates with rich text widget integration, images and a
smart queueing system all right from the admin interface.

Status
======
We are currently using this package in several large to medium scale
production environments, but it should be considered a permanent work in
progress.


Compatibility
=============
Currently, BulletShotNews is being tested to run on Python 2.5-2.7 and the
latest Django 1.3 and 1.4 releases. Django 1.5 support is underway.

Requirements
============
Please refer to `requirements.txt <http://github.com/marcelomilo/BulletShotNews/blob/master/requirements.txt>`_ for an updated list of required packes.

Installation
============
#)  Get it from the Cheese Shop::

  pip install BulletShotNews

    **Or** get the latest & greatest from Github and link it to your
    application tree::

	pip install -e git://github.com/marcelomilo/BulletShotNews.git#egg=BulletShotNews

    (In either case it is recommended that you use
    `VirtualEnv <http://pypi.python.org/pypi/virtualenv>`_ in order to
    keep your Python environment somewhat clean.)

#)  Add newsletter and to ``INSTALLED_APPS`` in settings.py and make sure that
    your favourite rich text widget (optional), some Django contrib dependencies,
    `sorl-thumbnail <http://sorl-thumbnail.readthedocs.org/en/latest/installation.html>`_
    and `django-extensions <https://github.com/django-extensions/django-extensions>`_
    (the latter is used for the submission jobs) are there as well::

	INSTALLED_APPS = (
	    'django.contrib.contenttypes',
	    'django.contrib.sessions',
	    'django.contrib.auth',
	    'django.contrib.sites',
	    ...
	    # Imperavi (or tinymce) rich text editor is optional
	    'imperavi',
	    'django_extensions',
	    'sorl.thumbnail',
	    ...
	    'newsletter',
	    ...
	)

#)  Install and configure your preferred rich text widget (optional).

    Known to work are `django-imperavi <http://pypi.python.org/pypi/django-imperavi>`_
    as well as for `django-tinymce <http://pypi.python.org/pypi/django-tinymce>`_.
    Be sure to follow installation instructions for respective widgets. After
    installation, the widgets can be selected as follows::

	# Using django-imperavi
	NEWSLETTER_RICHTEXT_WIDGET = "imperavi.widget.ImperaviWidget"

	# Using django-tinymce
	NEWSLETTER_RICHTEXT_WIDGET = "tinymce.widgets.TinyMCE"

    If not set, django-newsletter will fall back to Django's default TextField
    widget.

#)  Import subscription, unsubscription and archive URL's somewhere in your
    `urls.py`::

	urlpatterns = patterns('',
	    ...
	    (r'^newsletter/', include('newsletter.urls')),
	    ...
	)

#)  Enable Django's `staticfiles <http://docs.djangoproject.com/en/dev/howto/static-files/>`_
    app so the admin icons, CSS and JavaScript will be available where
    we expect it.

#)  Create required data structure and load default template fixture::

	./manage.py syncdb
	./manage.py loaddata default_templates

#)  Change the default contact email listed in
    ``templates/newsletter/subscription_subscribe.html`` and
    ``templates/newsletter/subscription_update.html``.

#)  Run the tests to see if it all works::

	./manage.py test

    If this fails, please contact me!
    If it doesn't: that's a good sign, chap. You'll probably have yourself a
    working configuration!

#)  Add jobs for sending out mail queues to `crontab <http://linuxmanpages.com/man5/crontab.5.php>`_::

	@hourly /path/to/my/project/manage.py runjobs hourly
	@daily /path/to/my/project/manage.py runjobs daily
	@weekly /path/to/my/project/manage.py runjobs weekly
	@monthly /path/to/my/project/manage.py runjobs monthly

South migrations / upgrading
============================
Since 5f79f40, the app makes use of `South <http://south.aeracode.org/>`_ for
schema migrations. As of this version, using South with BulletShotNews
is the official recommendation and `installing it <http://south.readthedocs.org/en/latest/installation.html>`_ is easy.

When upgrading from a pre-South version of Bullet News to a current
release (in a project for which South has been enabled), you might have to
fake the initial migration as the DB tables already exist. This can be done
by running the following command::

	./manage.py migrate newsletter 0001 --fake

Usage
=====
#) Start the development server: ``./manage.py runserver``
#) Navigate to ``/admin/`` and: behold!
#) Put a submission in the queue.
#) Submit your message with ``./manage.py runjob submit``


Unit tests
==========
Fairly extensive tests are available for internal frameworks, web
(un)subscription and mail sending. Sending a newsletter to large groups of recipients
(+10k) has been confirmed to work in multiple production environments. Tests
for pull req's and the master branch are automatically run through
`Travis CI <http://travis-ci.org/marcelomilo/BulletShotNews>`_.

Feedback
========
If you find any bugs or have feature request for django-newsletter, don't hesitate to
open up an issue on `GitHub <https://github.com/marcelomilo/BulletShotNews/issues>`_
(but please make sure your issue hasn't been noticed before, finding duplicates is a
waste of time). When modifying or adding features to django-newsletter in a fork, be
sure to let me know what you're building and how you're building it. That way we can
coordinate whether, when and how it will end up in the main fork and (eventually) an
official release.

In general: thanks for the support, feedback, patches and code that's been flowing in
over the years! Django has a truly great community. <3

License
=======
