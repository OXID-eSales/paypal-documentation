PayPal Express for GraphQL
==========================

The PayPal Express payment method can be used via the OXID Grapqhl API. Requirement is that the
`OXID eSales GraphQL Storefront Module <https://github.com/OXID-eSales/graphql-storefront-module>`__,
is installed and activated along side the PayPal module.

To check out with PayPal Express, a user does not need a user account in the OXID eShop, he
can be anonymous up to the point where the order gets placed. At placing order point in the checkout process,
he either gets matched to an already existing account in the OXID eShop or a new customer (without password)
is created. This is based on the information the shop gets from PayPal.
Of course, the user can also be logged in to the OXID eShop when placing an order with PayPal Express.
We need to cover both possibilities when checking out with PayPal via the GraphQL API.

As the  OXID GraphQL API does not use a session, the way to identify a user is via his GraohQL JWT token.
An anonymus user needs to get an anonymous token to be able to prepare a basket.


PayPal Express checkout workflow via GraphQL API
------------------------------------------------

In order to successfully place an order via the GraphQL API using PayPal Express checkout, you need to

- create a basket and fill with products
- communicate with PayPal API to register an express checkout transaction and get token and approval url
- have the end user visit the approval url and approve the transaction with PayPal
- optionally query the PayPal token status (to get information, if user still needs to confirm the order)
- and finally place the order

.. uml::
   :align: center

   "Client (PWA)" -> "Base GraphQL API": token query (JWT for anonymous or existing user)
   "Client (PWA)" -> "Storefront GraphQL API": basketCreate mutation
   "Client (PWA)" <- "Storefront GraphQL API": Basket datatype or error
   "Client (PWA)" -> "Storefront GraphQL API": basketAddProduct mutation
   "Client (PWA)" <- "Storefront GraphQL API": Basket datatype or error
   "Client (PWA)" -> "PayPal GraphQL API": paypalExpressApprovalProcess query
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
   :caption: call to ``paypalExpressApprovalProcess`` query

        query {
            paypalExpressApprovalProcess(
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
   :caption: ``paypalExpressApprovalProcess`` query response

        {
            "data": {
                "paypalExpressApprovalProcess": {
                    "token": "EC-40C8419686197933V",
                    "communicationUrl": "https://www.sandbox.paypal.com/cgi-bin/webscr&cmd=_express-checkout&token=EC-40C8419686197933V&useraction=continue"
                }
            }
        }

.. code-block:: graphql
   :caption: call to ``paypalTokenStatus`` query

        query  {
            paypalTokenStatus(
                paypalToken: "EC-40C8419686197933V"
            ) {
               token
               tokenApproved
            }
        }

.. code-block:: json
   :caption: ``paypalTokenStatus`` query response

        {
          "data": {
            "paypalTokenStatus": {
              "token": "EC-40C8419686197933V",
              "tokenApproved": true
            }
          }
        }

Placing the order
-----------------

During  ``placeOrder mutation`` the ``OxidEsales\GraphQL\Storefront\Basket\Event\BeforePlaceOrder`` event is dispatched
and handled by PayPal. Right before the order gets finalized, all requirements for a PayPal payment are validated:

   *  PayPal token must be valid and approved
   *  basket amount must not have increased after customer approved Payment with PayPal
   *  delivery address and delivery method are taken from PayPal information. In case of an anonymous user,
      information from PayPal is matched against existing shop user accounts. A new user account is created
      or a delivery address is added to an existing user account if necessary.

The PaymentGateway takes care about the payment via PayPal like it does for the non GraphQL checkout workflow.