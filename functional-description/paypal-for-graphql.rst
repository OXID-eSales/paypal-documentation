PayPal for GraphQL
==================

The PayPal payment method can be used via the OXID Grapqhl API. Requirements is that the
`OXID eSales GraphQL Storefront Module <https://github.com/OXID-eSales/graphql-storefront-module>`__,
is installed and activated along side the PayPal module.

To gain some knowledge about the checkout process via the GraphQL API, please check the
`GraphQL documentation <https://docs.oxid-esales.com/interfaces/graphql/en/5.1/>`__, especially the section
about placing an order `placing an order <https://docs.oxid-esales.com/interfaces/graphql/en/5.1/consuming/PlaceOrder.html>`__.

PayPal checkout workflow via GraphQL API
----------------------------------------

In order to successfully place an order via the GraphQL API you need to

- create a basket and fill with products
- set a delivery address (in case it is different from invoice address)
- set desired delivery option
- set desired payment option (PayPal)
- communicate with PayPal API to setExpressCheckout and get token and approval url
- have the end user visit the approval url and approve the transaction with PayPal
- optionally query the PayPal token status (to get information, if user still needs to confirm the order)
- and finally place the order

.. uml::
   :align: center

   "Client (PWA)" -> "Storefront GraphQL API": basketCreate mutation
   "Client (PWA)" <- "Storefront GraphQL API": Basket datatype or error
   "Client (PWA)" -> "Storefront GraphQL API": basketAddProduct mutation
   "Client (PWA)" <- "Storefront GraphQL API": Basket datatype or error
   "Client (PWA)" -> "Storefront GraphQL API": basketDeliveryMethods query
   "Client (PWA)" <- "Storefront GraphQL API": array of DeliveryMethod data types
   "Client (PWA)" -> "Storefront GraphQL API": basketSetDeliveryMethod mutation
   "Client (PWA)" <- "Storefront GraphQL API": Basket datatype or error
   "Client (PWA)" -> "Storefront GraphQL API": basketPayments query
   "Client (PWA)" <- "Storefront GraphQL API": array of Payment data types
   "Client (PWA)" -> "Storefront GraphQL API": basketSetPayment mutation
   "Client (PWA)" <- "Storefront GraphQL API": Basket datatype or error
   "Client (PWA)" -> "PayPal GraphQL API": paypalApprovalProcess query
   "Client (PWA)" <- "PayPal GraphQL API": PayPalCommunicationInformation or error
   "Client (PWA)" -> "PayPal GraphQL API": paypalTokenStatus query (optional)
   "Client (PWA)" <- "PayPal GraphQL API": PayPalTokenStatus or error (optional)
   "Client (PWA)" -> "Storefront GraphQL API": placeOrder mutation
   "Client (PWA)" <- "Storefront GraphQL API": Order datatype or error

The standard checkout functionality is part of the
`OXID eSales GraphQL Storefront Module <https://github.com/OXID-eSales/graphql-storefront-module>`__,
the PayPal specific queries are built into the PayPal module.

Prepare for PayPal approval
---------------------------

.. code-block:: graphql
   :caption: call to ``paypalApprovalProcess`` query

        query {
            paypalApprovalProcess(
                basketId: "51cb902ea2735b5e1b08f4b11b1c5f6f",
                returnUrl: "http://www.oxid-eshop.local/returnFromPayPal",
                cancelUrl: "http://www.oxid-eshop.local/cancelPayPal",
                displayBasketInPayPal: true
            ) {
               token
               communicationUrl
            }
        }

.. code-block:: json
   :caption: ``paypalApprovalProcess`` query response

        {
            "data": {
                "paypalApprovalProcess": {
                    "token": "EC-0YG60371V1312494K",
                    "communicationUrl": "https://www.sandbox.paypal.com/cgi-bin/webscr&cmd=_express-checkout&token=EC-0YG60371V1312494K&useraction=continue"
                }
            }
        }

.. code-block:: graphql
   :caption: call to ``paypalTokenStatus`` query

        query  {
            paypalTokenStatus(
                paypalToken: "EC-1GP49836TE460842X"
            ) {
               token
               status
            }
        }

.. code-block:: json
   :caption: ``paypalTokenStatus`` query response

        {
          "data": {
            "paypalTokenStatus": {
              "token": "EC-1GP49836TE460842X",
              "status": true
            }
          }
        }

Placing the order
-----------------

During  ``placeOrder mutation`` the ``OxidEsales\GraphQL\Storefront\Basket\Event\BeforePlaceOrder`` event is dispatched
and handled by PayPal. Right before the order gets finalized, all requirements for a PayPal payment are validated:

   *  token must be valid and approved
   *  delivery address must be the same as registered with PayPal
   *  basket amount must not have increased after customer approved Payment with PayPal

The PaymentGateway takes care about the payment via PayPal like it does for the non GraphQL checkout workflow.