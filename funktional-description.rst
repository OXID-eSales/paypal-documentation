Functional description
======================

In the OXID eShop, orders can be paid via PayPal.

PayPal in the checkout process
------------------------------
With PayPal Basis, PayPal can be selected as payment method during checkout step 3. At this time, the user is already logged in to the shop or shopping without registration.

.. image:: media/screenshots-en/oxdaah01.png
    :alt: Checkout, order step 3
    :class: with-shadow
        :height: 648
        :width: 650

    When checking out with PayPal Express, the buyer can complete the purchase from checkout step 1, from checkout step 2 if not yet logged in, the product details page or the mini cart.

.. image:: media/screenshots-en/oxdaah02.png
    :alt: Checkout, order step 1
    :class: with-shadow
        :height: 383
        :width: 650

|br|

.. image:: media/screenshots-en/oxdaah03.png
    :alt: Checkout, order step 2
    :class: with-shadow
        :height: 305
        :width: 650

|br|

.. image:: media/screenshots-en/oxdaah04.png
    :alt: Product details page with mini cart
    :class: with-shadow
        :height: 352
        :width: 650

|br|
In both cases, the user will be directed to the PayPal payment page. According to the configuration and customer's approval, the ordered products are shown on the PayPal payment page.

.. hint:: If there are products with a fraction of a quantity (e.g. 1,5) in the shopping cart, the shopping cart will not be submitted to PayPal, no matter if the purchaser activated this option during the checkout.

.. image:: media/screenshots-en/oxdaah05.png
    :alt: PayPal payment page
    :class: with-shadow
        :height: 464
        :width: 650

    The customer can now login to his PayPal account or create a new account. After payment confirmation, the customer is redirected to the shop.

    In case of express order, also the customer's information from the PayPal account is provided to the shop. Thus, the form in checkout step 2 does not need to be filled out. Since also the shipping method can be selected on the PayPal payment page, the checkout process jumps directly to step 4.

    After checkout is completed, the payment is arranged. Depending on the configuration, the amount is either transferred immediately between the PayPal accounts or the payment is authorized. The authorized amount will be captured manually at a later time.

--------------------------------------------------

PayPal for orders
-----------------
If products have been paid via PayPal, payment information, a PayPal history and an overview of the ordered products will be shown on the :guilabel:`PayPal` tab of the order in the admin panel.

.. image:: media/screenshots-en/oxdaah06.png
    :alt: Orders, "PayPal" tab
    :class: with-shadow
        :height: 325
        :width: 650

    The payment information shows the payment status, the total amount of the order and amounts that were captured, canceled or refunded.

    In case of delayed capture of an order amount (AUTH), up to 10 captures can be made within a period of 29 days. This allows you to react flexibly if, for example, only parts of an order can be fulfilled at a given time. In this case, we recommend capturing the first partial deliver immediately after the order is completed and authorization is made. Then wait until all remaining items of the order are available for shipment and capture the remaining amount.

    With a click on the :guilabel:`Capture` button, the total amount or partial amounts can be captured from the customer account. This process can be documented by a comment.

A granted authorization for capture can be canceled and a payment status can be set. The payment status can be "Completed", "Pending" or "Canceled". Also here it is possible to document, for example, the reason for the cancellation by a comment.


In the :guilabel:`PayPal history`, all transactions are shown in a summarize table. For each transaction such as authorization, capture, refund or cancellation, a line is created in the table at the end of which further details can be viewed by clicking on a small button. The table lines for the capture of an amount have another button for refunds. Thus, the refund can be assigned exactly to an amount captured.

An additional table on the tab gives an overview of all ordered products including quantity, product number, product name, price and VAT.

.. Intern: oxdaah, Status: