Security
========

AnonymousVoter
--------------

``AnonymousVoter`` only allows anonymous users or non-authenticated users access to a resource.

.. code-block:: php

    <?php
    $accessDecisionManager = new AccessDecisionManager(array(
        new AnonymousVoter(),
    ));

If you are using Symfony2 and ``Resources/config/services.xml`` have been loaded into the dependency injection container.

.. code-block:: yaml

    # security.yml
    security:
        access_control:
            - { path: /login, roles: [IS_ANONYMOUS] }

BCrypt Encoder
--------------

We all know that BCrypt is the best and if you are not so sure about that see "`Use BCrypt Fool! <http://yorickpeterse.com/articles/use-bcrypt-fool>`_"

.. code-block:: php

    <?php

    $encoder = new BcryptPasswordEncoder($cost = 5);

    // Encode password. The second argument is the $salt which is ignored but
    // part of the PasswordEncoderInterface definition
    $encoder->encodePassword('password', null);

If you are using Symfony2 and ``Resources/config/services.xml`` have been loaded into the dependency injection container.

.. code-block:: yaml

    # security.yml
    security:
        encoders:
            Vandpibe\VandpibeBundle\Model\User: { id: vandpibe.security.encoder.bcrypt }
