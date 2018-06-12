Installation
============

In this chapter, the installation of the PayPal module for the OXID eShop versions 6.0 is described.

System requirements
-------------------
The system requirements given below apply to the PayPal module. In addition, the PayPal module only works if the OXID eShop was configured for SSL mode.

* PHP 5.6 or higher
* cURL
* OpenSSL

.. |step| image:: media/icons/schritt.jpg

--------------------------------------------------

New installation
----------------
If the shop was set up as an OXID eShop Compilation, the matching version of OXID eFire Extension PayPal is already integrated. It simply has to be activated and configured via the admin panel.

|step| Installing the PayPal module
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
# ToDo: Übersetzung prüfen

In case that the module has to be manually integrated into the shop it has to be downloaded from the repository and installed. Therefore the following Composer command is used in the shell opened in the root directory of the shop:

.. code:: bash

  composer require oxid-esales/paypal-module:^5.0.0

|step| Setting permissions for the logging
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Change the write permission of :file:`/modules/oe/oepaypal/logs`. Grant full write permission for owner, group and public (755 or 777).

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
In case you have to update the module separately delete all files and folders in :file:`/source/modules/oe/oepaypal`.

|step| Installing the PayPal module
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Install the current module using the following Composer command in the shell which was opened in the root directory of the shop:

.. code:: bash

  composer require oxid-esales/paypal-module:^5.0.0

|step| Finishing update
^^^^^^^^^^^^^^^^^^^^^^^
Finish the update installation - like it is described below - by setting the permissions for the logging, activating the module in the admin area and deleting the temporary files.

.. Intern: oxdaaf, Status: