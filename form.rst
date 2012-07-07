Form
====

NullToDateTimeTransformer
-------------------------

When binding a form with ``null`` and the form have a ``date`` type field it will try and pass ``null`` to the model.
This is solved by converting ``null`` into a ``DateTime`` using the current timestamp.

.. code-block:: php

    <?php

    use Vandpibe\Form\DataTransformer\NullToDateTimeTransformer;

    $builder->get('dateField')->addModelTransformer(new NullToDateTimeTransformer());

This can be cumbersome to do for every date field if you want it system wide. Therefor there is also a ``FormTypeExtensionInterface``
implementation which can be used by adding ``Resources/config/services.xml`` to your Dependency Injection container. If it is absolutely
necesarry it can be used standalone.

.. code-block:: php

    <?php

    use Vandpibe\Form\Extension\DateTypeExtension;

    $extension = new DateTypeExtension();

EncoderTransfomer
-----------------

``EncoderTransformer`` will automatically encode the value that is being reverse transformer with the correct
``PasswordEncoderInterface`` through ``EncoderFactoryInterface``.

.. code-block:: php

    <?php

    $builder->get('password')->addModelTransformer(new EncoderTransformer($encoderFactory, $user));

AuthenticationSubscriber
------------------------

When using the full stack Symfony2 framework the Security login form is not rendered as a form. This can luckily be changed
quite easily.

.. code-block:: php

    <?php

    // Assuming you have access to the $session object and a $formBuilder instance
    $subscriber = new AuthenticationSubscriber($session);
    $formBuilder->addEventSubscriber($subscriber);

AuthenticationType
------------------

Can be used as a replacement for doing the same security check for password etc.

.. code-block:: php

    <?php

    class SecurityController extends Controller
    {
        public function loginAction(Request $request)
        {
            $form = $this->createForm(new AuthenticationType($request->getSession()));

            return $this->render('SomeBundle:Security:login.html.twig', array(
                'form' => $form->createView(),
            ));
        }
    }
