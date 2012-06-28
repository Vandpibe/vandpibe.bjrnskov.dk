Validator
=========

Blacklist Validator
-------------------

`Blacklist` is a simple constraint and validator for Symfony Validator component. It makes sure the submitted
value is not part of the blacklist.

A blacklist can be useful for making sure your urls are not used as usernames. `See more on Quora <http://www.quora.com/How-do-sites-prevent-vanity-URLs-from-colliding-with-future-features>`_

.. code-block:: php

    <?php
    $constraint = new Blacklist(array(array(
        'Blacklist 1',
        'Blacklist 2',
    ));

    $validator = new BlacklistValidator();
    $validator->initialize($context);
    $validator->validate('value', $constraint);

It can also be used with Symfony2 validation config as a Annotation like.

.. code-block:: php

    <?php

    use Vandpibe\Validator\Constraint\Blacklist;

    class Entity
    {
        /**
         * @Blacklist({ "blacklist 1", "blacklist 2" })
         */
        protected $var;
    }


