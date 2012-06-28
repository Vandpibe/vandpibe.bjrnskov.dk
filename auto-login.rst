AutoLogin
=========

AutoLogin build upon another great library `Jmikola AutoLogin <http://github.com/jmikola/autologin>`_. Which adds
AutoLogin capabilities to Symfony's Security component.

AutoLogin adds generation of secure auto-login urls. The urls generated is base64 encoded and looks like this

.. code-block:: text

    c29tZXRoaW5nbGlrZXRoaXNpc2F3ZXNvbWVhbmR2ZXJ5dmVybG9uZzkwNzI4OTYyNzR0aWhhZGs3OTAyMzk0Mjk4NzIz


Basic usage
-----------

.. code-block:: php

    <?php

    use Vandpibe\AutoLogin\Generator;
    use Vandpibe\AutoLogin\Hasher;
    use Symfony\Component\Security\Core\User\UserInterface;

    $user = new User();

    $hasher = new Hasher('sharedsecret');
    $generator = new Generator($hasher);

    print $generator->generate($user);

Twig Extension
--------------

As auto-login's mostly are done from templates there is a Twig extension which helps with generating the full url.
The extension requires Symfony's Routing component for generating the actual url.

.. code-block:: php

    <?php

    $twig = new Twig_Environment();
    $twig->addExtension(new AutoLoginExtension($generator, $urlGenerator));

.. code-block:: jinja

    {# Returns an absolute url with the auto_login generated string appended #}
    <a href="{{ auto_login_url(app.user, 'vandpibe_user_edit') }}">Edit profile</a>

    {# Returns the auto_login generated string. #}
    {{ auto_login(user, 86400) }}

Silex and Cilex integration
---------------------------

For Silex and Cilex there is a ServiceProvider. Which adds a Generator, Hasher and enables the TwigExtension if Twig is available.

.. code-block:: php

    <?php
    use Silex\Application;
    use Vandpibe\AutoLogin\Silex\AutoLoginServiceProvider;

    $app = new Application();
    $app->register(new AutoLoginServiceProvider(), array(
        'vandpibe.auto_login.secret' => 'a-very-shared-secret',
    ));

.. note::

    For the Cilex version just substitute Silex for Cilex in the above example

Jmikola AutoLogin
-----------------

Jmikola's AutoLogin componenent have a ``AutoLoginUserProviderInterface``. ``Vandpibe\AutoLogin\Security\UserProvider`` is a concrete
implementation of that which wraps a normal ``Symfony\Component\Security\User\Core\UserProviderInterface`` implementation.

.. code-block:: php

    <?php

    use Vandpibe\AutoLogin\UserProvider;
    use Vandpibe\AutoLogin\Generator;
    use Symfony\Component\Security\Core\User\UserProviderInterface;

    $provider = new UserProvider(UserProviderInterface $provider, Generator $generator);
