Installation
============

1. Install package:

.. code-block:: python

    pip install django-rest-auth

2. Add ``rest_auth`` app to INSTALLED_APPS in your django settings.py:

.. code-block:: python

    INSTALLED_APPS = (
        ...,
        'rest_framework',
        'rest_framework.authtoken',
        ...,
        'rest_auth'
    )


.. note:: This project depends on ``django-rest-framework`` library, so install it if you haven't done yet. Make sure also you have installed ``rest_framework`` and ``rest_framework.authtoken`` apps

3. Add rest_auth urls:

.. code-block:: python

    urlpatterns = patterns('',
        ...,
        url(r'^rest-auth/', include('rest_auth.urls'))
    )


You're good to go now!


Registration (optional)
-----------------------

1. If you want to enable standard registration process you will need to install ``django-allauth`` by using ``pip install django-rest-auth[extras]`` or ``pip install django-rest-auth[with_social]``.

2. Add ``allauth``, ``allauth.account`` and ``rest_auth.registration`` apps to INSTALLED_APPS in your django settings.py:

.. code-block:: python

    INSTALLED_APPS = (
        ...,
        'allauth',
        'allauth.account',
        'rest_auth.registration',
    )

3. Add rest_auth.registration urls:

.. code-block:: python

    urlpatterns = patterns('',
        ...,
        url(r'^rest-auth/', include('rest_auth.urls')),
        url(r'^rest-auth/registration/', include('rest_auth.registration.urls'))
    )


Social Authentication (optional)
--------------------------------

Using ``django-allauth``, ``django-rest-auth`` provides helpful class for creating social media authentication view. Below is an example with Facebook authentication.

.. note:: Points 1, 2 and 3 are related with ``django-allauth`` configuration, so if you have already configured social authentication, then please go to step 4. See ``django-allauth`` documentation for more details.

1. Add ``allauth.socialaccount`` and ``allauth.socialaccount.providers.facebook`` apps to INSTALLED_APPS in your django settings.py:

.. code-block:: python

    INSTALLED_APPS = (
        ...,
        'rest_framework',
        'rest_framework.authtoken',
        'rest_auth'
        ...,
        'allauth',
        'allauth.account',
        'rest_auth.registration',
        ...,
        'allauth.socialaccount',
        'allauth.socialaccount.providers.facebook',
    )

2. Add Social Application in django admin panel

3. Create new view as a subclass of ``rest_auth.registration.views.SocialLoginView`` with ``FacebookOAuth2Adapter`` adapter as an attribute:

.. code-block:: python

    from allauth.socialaccount.providers.facebook.views import FacebookOAuth2Adapter
    from rest_auth.registration.views import SocialLoginView

    class FacebookLogin(SocialLoginView):
        adapter_class = FacebookOAuth2Adapter

4. Create url for FacebookLogin view:

.. code-block:: python

    urlpatterns += pattern('',
        ...,
        url(r'^rest-auth/facebook/$', FacebookLogin.as_view(), name='fb_login')
    )

.. note:: Starting from v0.21.0, django-allauth has dropped support for context processors. Check out http://django-allauth.readthedocs.org/en/latest/changelog.html#from-0-21-0 for more details.
