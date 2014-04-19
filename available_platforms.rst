Available Platforms
===================

These are the platforms currently supported by the main PaymentSuite team.
Each platform has been developed as a wrapper of PaymentCoreBundle


AuthorizenetBundle
-------------
This bundle bring you a possibility to make simple payments through `Authorize.Net <http://www.authorize.net/>`_.

Install
~~~~~~~~~~~~~

You have to add require line into you composer.json file

.. code-block:: yaml

    "require": {
       // ...
       "paymentsuite/authorizenet-bundle": "1.1"
    }

Then you have to use composer to update your project dependencies

.. code-block:: bash

    $ curl -sS https://getcomposer.org/installer | php
    $ php composer.phar update paymentsuite/authorizenet-bundle

And register the bundle in your ``AppKernel.php`` file

.. code-block:: php

    return array(
       // ...
       new PaymentSuite\PaymentCoreBundle\PaymentCoreBundle(),
       new PaymentSuite\AuthorizenetBundle\AuthorizenetBundle(),
    );

Configuration
~~~~~~~~~~~~~

If it's first payment method of PaymentSuite in your project, first you have to configure PaymentBridge Service and Payment Event Listener following `this documentation <http://docs.paymentsuite.org/en/latest/configuration.html/>`_.

Configure the AuthorizenetBundle parameters in your ``config.yml``.

.. code-block:: yaml

    authorizenet:

       # authorizenet keys
       login_id: XXXXXXXXXXXX
       tran_key: XXXXXXXXXXXX
       test_mode: true
       
       # By default, controller route is /payment/authorizenet/execute
       controller_route: /my/custom/route
       
       # Configuration for payment success redirection
       #
       # Route defines which route will redirect if payment success
       # If order_append is true, Bundle will append cart identifier into route
       #    taking order_append_field value as parameter name and
       #    PaymentOrderWrapper->getOrderId() value
       payment_success:
          route: cart_thanks
          order_append: true
          order_append_field: order_id
       
       # Configuration for payment fail redirection
       #
       # Route defines which route will redirect if payment fails
       # If cart_append is true, Bundle will append cart identifier into route
       #    taking cart_append_field value as parameter name and
       #    PaymentCartWrapper->getCartId() value
       payment_fail:
          route: cart_view
          cart_append: false
          cart_append_field: cart_id

About Authorizenet ``login_id`` and ``tran_key`` you can learn more in 
`Authorizenet documentation page <http://support.authorize.net/authkb/index?page=content&id=A576&actp=LIST_POPULAR>`_.

Router
~~~~~~

AuthorizenetBundle allows developer to specify the route of controller where Authorize.Net callback is processed.
By default, this value is ``/payment/authorizenet/callback`` but this value can be changed in configuration file.
Anyway AuthorizenetBundle's routes must be parsed by the framework, so these lines must be included into ``routing.yml`` file.

.. code-block:: yaml

    authorizenet_payment_routes:
       resource: .
       type: authorizenet

Display
~~~~~~~

Once your AuthorizenetBundle is installed and well configured, you need to place your payment form.

AuthorizenetBundle gives you all form view as requested by the payment module.

.. code-block:: twig

    {% block content %}
       <div class="payment-wrapper">
          {{ authorizenet_render() }}
       </div>
    {% endblock content %}

Customize
~~~~~~~~~

``authorizenet_render()`` just print a basic form.

As every project need its own form design, you can overwrite default form located in: ``app/Resources/AuthorizenetBundle/views/Authorizenet/view.html.twig``.

Testing and more documentation
~~~~~~~~~

For testing you can use these example `these examples <http://developer.authorize.net/testingfaqs/>`_. More detail about Authorizenet API you can find in this `web <http://developer.authorize.net/>`_.


PaymillBundle
-------------

Configuration
~~~~~~~~~~~~~

Configure the PaymillBundle configuration in your ``config.yml``

.. code-block:: yaml

    paymill:

        # paymill keys
        public_key: XXXXXXXXXXXX
        private_key: XXXXXXXXXXXX

        # By default, controller route is /payment/paymill/execute
        controller_route: /my/custom/route

        # Configuration for payment success redirection
        #
        # Route defines which route will redirect if payment successes
        # If order_append is true, Bundle will append card identifier into route
        #    taking order_append_field value as parameter name and
        #    PaymentOrderWrapper->getOrderId() value
        payment_success:
            route: card_thanks
            order_append: true
            order_append_field: order_id

        # Configuration for payment fail redirection
        #
        # Route defines which route will redirect if payment fails
        # If card_append is true, Bundle will append card identifier into route
        #    taking card_append_field value as parameter name and
        #    PaymentCardWrapper->getCardId() value
        payment_fail:
            route: card_view
            card_append: false
            card_append_field: card_id

Extra Data
~~~~~~~~~~

PaymentBridge Service must return, at least, these fields.

- order_description

Router
~~~~~~

PaymillBundle allows developer to specify the route of controller where paymill
payment is processed.
By default, this value is ``/payment/paymill/execute`` but this value can be
changed in configuration file.
Anyway, the bundle routes must be parsed by the framework, so these lines must
be included into routing.yml file

.. code-block:: yaml
    paymill_payment_routes:
        resource: .
        type: paymill`

Display
~~~~~~~

Once your Paymill is installed and well configured, you need to place your
payment form.

PaymillBundle gives you all form view as requested by the payment module.

.. code-block:: twig
    {% block content %}
            <div class="payment-wrapper">
                {{ paymill_render() }}
            </div>
    {% endblock content %}

    {% block foot_script %}
        {{ parent() }}
        {{ paymill_scripts() }}
    {% endblock foot_script %}

Customize
~~~~~~~~~

``paymill_render()`` only print form in a simple way.

As every project need its own form design, you should overwrite in
``app/Resources/PaymillBundle/views/Paymill/view.html.twig``, paymill form render
template placed in
``PaymentSuite/Paymill/Bundle/Resources/views/Paymill/view.html.twig``.


GoogleWalletBundle
-------------
This bundle bring you a possibility to make simple payments through `Google Wallet <http://www.google.com/wallet/>`_.

Install
~~~~~~~~~~~~~

You have to add require line into you composer.json file

.. code-block:: yaml

    "require": {
       // ...
       "paymentsuite/google-wallet-bundle": "1.1"
    }

Then you have to use composer to update your project dependencies

.. code-block:: bash

    $ curl -sS https://getcomposer.org/installer | php
    $ php composer.phar update paymentsuite/google-wallet-bundle

And register the bundle in your ``AppKernel.php`` file

.. code-block:: php

    return array(
       // ...
       new PaymentSuite\PaymentCoreBundle\PaymentCoreBundle(),
       new PaymentSuite\GoogleWalletBundle\GoogleWalletBundle(),
    );

Configuration
~~~~~~~~~~~~~

If it's first payment method of PaymentSuite in your project, first you have to configure PaymentBridge Service and Payment Event Listener following `this documentation <http://docs.paymentsuite.org/en/latest/configuration.html/>`_.

Configure the GoogleWalletBundle parameters in your ``config.yml``.

.. code-block:: yaml

    google_wallet:

        # google wallet keys
        merchant_id: XXXXXXXXXXXX
        secret_key: XXXXXXXXXXXX

        # Configuration for payment success redirection
        #
        # Route defines which route will redirect if payment success
        # If order_append is true, Bundle will append cart identifier into route
        #    taking order_append_field value as parameter name and
        #    PaymentOrderWrapper->getOrderId() value
        payment_success:
            route: cart_thanks
            order_append: true
            order_append_field: order_id

        # Configuration for payment fail redirection
        #
        # Route defines which route will redirect if payment fails
        # If cart_append is true, Bundle will append cart identifier into route
        #    taking cart_append_field value as parameter name and
        #    PaymentCartWrapper->getCartId() value
        payment_fail:
            route: cart_view
            cart_append: false
            cart_append_field: cart_id

To get ``merchant_id`` and ``secret_key`` you have to register for `Sandbox Settings <https://sandbox.google.com/checkout/inapp/merchant/settings.html>`_ or `Production Settings <https://checkout.google.com/inapp/merchant/settings.html>`_. Also there you have to set postback URL (must be on public DNS and not localhost). For more information you can visit page of `Google Wallet APIs <https://developers.google.com/wallet/>`_.

Extra Data
~~~~~~~~~~

PaymentBridge Service must return, at least, these fields.

- order_name
- order_description

Router
~~~~~~

GoogleWalletBundle allows developer to specify the route of controller where Google Wallet callback is processed.
By default, this value is ``/payment/googlewallet/callback`` but this value can be changed in configuration file.
Anyway GoogleWalletBundle's routes must be parsed by the framework, so these lines must be included into ``routing.yml`` file.

.. code-block:: yaml

    google_wallet_payment_routes:
        resource: .
        type: googlewallet

Display
~~~~~~~

Once your GoogleWalletBundle is installed and well configured, you need to place submit button which open Google Wallet pop-up.

GoogleWalletBundle gives you all code as requested by the payment module.

.. code-block:: twig

    {% block content %}
        <div class="payment-wrapper">
            {{ googlewallet_render() }}
        </div>
    {% endblock content %}

    {% block foot_script %}
        {{ parent() }}
        {{ googlewallet_scripts() }}
    {% endblock foot_script %}

Customize
~~~~~~~~~

As every project need its own form design, you can overwrite default button located in: ``app/Resources/GoogleWalletBundle/views/GoogleWallet/view.html.twig``.

Testing and more documentation
~~~~~~~~~

For testing, you just have to use sandbox settings.
More details about Google Wallet API you can find in this `web <https://developers.google.com/wallet/>`_.

PaymillBundle
-------------

Configuration
~~~~~~~~~~~~~

Configure the PaymillBundle configuration in your ``config.yml``

.. code-block:: yaml

    paymill:

        # paymill keys
        public_key: XXXXXXXXXXXX
        private_key: XXXXXXXXXXXX

        # By default, controller route is /payment/paymill/execute
        controller_route: /my/custom/route

        # Configuration for payment success redirection
        #
        # Route defines which route will redirect if payment successes
        # If order_append is true, Bundle will append card identifier into route
        #    taking order_append_field value as parameter name and
        #    PaymentOrderWrapper->getOrderId() value
        payment_success:
            route: card_thanks
            order_append: true
            order_append_field: order_id

        # Configuration for payment fail redirection
        #
        # Route defines which route will redirect if payment fails
        # If card_append is true, Bundle will append card identifier into route
        #    taking card_append_field value as parameter name and
        #    PaymentCardWrapper->getCardId() value
        payment_fail:
            route: card_view
            card_append: false
            card_append_field: card_id

Extra Data
~~~~~~~~~~

PaymentBridge Service must return, at least, these fields.

- order_description

Router
~~~~~~

PaymillBundle allows developer to specify the route of controller where paymill
payment is processed.
By default, this value is ``/payment/paymill/execute`` but this value can be
changed in configuration file.
Anyway, the bundle routes must be parsed by the framework, so these lines must
be included into routing.yml file

.. code-block:: yaml
    paymill_payment_routes:
        resource: .
        type: paymill`

Display
~~~~~~~

Once your Paymill is installed and well configured, you need to place your
payment form.

PaymillBundle gives you all form view as requested by the payment module.

.. code-block:: twig
    {% block content %}
            <div class="payment-wrapper">
                {{ paymill_render() }}
            </div>
    {% endblock content %}

    {% block foot_script %}
        {{ parent() }}
        {{ paymill_scripts() }}
    {% endblock foot_script %}

Customize
~~~~~~~~~

``paymill_render()`` only print form in a simple way.

As every project need its own form design, you should overwrite in
``app/Resources/PaymillBundle/views/Paymill/view.html.twig``, paymill form render
template placed in
``PaymentSuite/Paymill/Bundle/Resources/views/Paymill/view.html.twig``.


StripeBundle
-------------
This bundle bring you a possibility to make simple payments through `Stripe <https://stripe.com>`_.

Install
~~~~~~~~~~~~~

You have to add require line into you composer.json file

.. code-block:: yaml

    "require": {
       // ...
       "paymentsuite/stripe-bundle": "1.1"
    }

Then you have to use composer to update your project dependencies

.. code-block:: bash

    $ curl -sS https://getcomposer.org/installer | php
    $ php composer.phar update paymentsuite/stripe-bundle

And register the bundle in your ``AppKernel.php`` file

.. code-block:: php

    return array(
       // ...
       new PaymentSuite\PaymentCoreBundle\PaymentCoreBundle(),
       new PaymentSuite\StripeBundle\StripeBundle(),
    );

Configuration
~~~~~~~~~~~~~

If it's first payment method of PaymentSuite in your project, first you have to configure PaymentBridge Service and Payment Event Listener following `this documentation <http://docs.paymentsuite.org/en/latest/configuration.html/>`_.

Configure the StripeBundle parameters in your ``config.yml``.

.. code-block:: yaml

    stripe:

        # stripe keys
        public_key: XXXXXXXXXXXX
        private_key: XXXXXXXXXXXX

        # By default, controller route is /payment/stripe/execute
        controller_route: /my/custom/route

        # Configuration for payment success redirection
        #
        # Route defines which route will redirect if payment success
        # If order_append is true, Bundle will append cart identifier into route
        #    taking order_append_field value as parameter name and
        #    PaymentOrderWrapper->getOrderId() value
        payment_success:
            route: cart_thanks
            order_append: true
            order_append_field: order_id

        # Configuration for payment fail redirection
        #
        # Route defines which route will redirect if payment fails
        # If cart_append is true, Bundle will append cart identifier into route
        #    taking cart_append_field value as parameter name and
        #    PaymentCartWrapper->getCartId() value
        payment_fail:
            route: cart_view
            cart_append: false
            cart_append_field: cart_id

About Stripe ``public_key`` and ``private_key`` you can learn more in `Stripe documentation page <https://stripe.com/docs/tutorials/dashboard#api-keys>`_.

Router
~~~~~~

StripeBundle allows developer to specify the route of controller where Stripe callback is processed.
By default, this value is ``/payment/stripe/callback`` but this value can be changed in configuration file.
Anyway StripeBundle's routes must be parsed by the framework, so these lines must be included into ``routing.yml`` file.

.. code-block:: yaml

    stripe_payment_routes:
        resource: .
        type: stripe

Display
~~~~~~~

Once your StripeBundle is installed and well configured, you need to place your payment form.

StripeBundle gives you all form view as requested by the payment module.

.. code-block:: twig

    {% block content %}
        <div class="payment-wrapper">
            {{ stripe_render() }}
        </div>
    {% endblock content %}

    {% block foot_script %}
        {{ parent() }}
        {{ stripe_scripts() }}
    {% endblock foot_script %}

Customize
~~~~~~~~~

`stripe_render()` just print a basic form.

As every project need its own form design, you can overwrite default form located in: ``app/Resources/StripeBundle/views/Stripe/view.html.twig`` following `Stripe documentation <https://stripe.com/docs/tutorials/forms>`_.

In another hand, Stripe `recommend  <https://stripe.com/docs/tutorials/forms#create-a-single-use-token>`_ use `jQuery form validator <https://github.com/stripe/jquery.payment>`_.

Testing and more documentation
~~~~~~~~~

For testing you can use `these examples <https://stripe.com/docs/testing>`_.
More detail about Stripe API you can find in this `web <https://stripe.com/docs/api/php>`_.
