Installation
============

In this chapter, the installation of the PayPal module for the OXID eShop version 6.2 is described.

System requirements
-------------------
The system requirements given below apply to the PayPal module. In addition, the PayPal module only works if OXID eShop was configured for SSL mode.

* PHP 7.0, 7.1 and 7.2
* cURL
* OpenSSL

.. |step| image:: media/icons/schritt.jpg

--------------------------------------------------

New installation
----------------
If the shop was set up as OXID eShop Compilation, the matching version of the PayPal module is already integrated. It simply has to be activated and configured via the admin panel.

|step| Installing the PayPal module
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In case that the module has to be manually integrated into the shop, it has to be downloaded from the repository and installed. Therefore, the following Composer command has to be used on the command line at the root directory of the shop:

.. code:: bash

  composer require oxid-esales/paypal-module:^6.0.0

|step| Setting permissions for the logging
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Change the permission of :file:`/modules/oe/oepaypal/logs`. Grant full write permission for owner and group (755 or 777).

|step| Activating the module
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The PayPal module has to be activated in the shop. In the :guilabel:`Overview` tab of the module, click the :guilabel:`Activate` button.

|step| Deleting temporary files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Delete all files and folders except for :file:`.htaccess` from the :file:`/tmp` folder of your shop.

--------------------------------------------------

Update installation
-------------------

|step| Deleting module files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In case you have to update the module separately, delete all files and folders in :file:`/source/modules/oe/oepaypal`.

|step| Installing the PayPal module
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Install the current module using the following Composer command on the command line at the root directory of the shop:

.. code:: bash

  composer require oxid-esales/paypal-module:^6.0.0

|step| Finishing update
^^^^^^^^^^^^^^^^^^^^^^^
Finish the update installation (like described above) by setting the permissions for the logging, activating the module in the admin panel and by deleting the temporary files.


.. Intern: oxdaab, Status: