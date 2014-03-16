Available Platforms
===================

These are the platforms currently supported by the main PaymentSuite team.
Each platform has been developed as a wrapper of PaymentCoreBundle

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