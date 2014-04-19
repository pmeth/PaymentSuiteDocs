Available Platforms
===================

These are the platforms currently supported by the main PaymentSuite team.
Each platform has been developed as a wrapper of PaymentCoreBundle


GoogleWalletBundle
-------------
This bundle bring you a possibility to make simple payments through `Google Wallet <http://www.google.com/wallet/>`_.

Install
~~~~~~~~~~~~~

You have to add require line into you composer.json file

.. code-block:: yaml

    "require": {
       // ...
       "paymentsuite/google-wallet-bundle": "1.0.1"
    }

Then you have to use composer to update your project dependencies

.. code-block:: bash

    $ curl -sS https://getcomposer.org/installer | php
    $ php composer.phar update

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

Extra Data
~~~~~~~~~~

PaymentBridge Service must return, at least, these fields.

- order_name
- order_description

Router
~~~~~~

GoogleWalletBundle allows developer to specify the route of controller where google wallet callback is processed.
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
