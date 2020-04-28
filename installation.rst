Installation
============

Dieses Dokument beschreibt die Installation des Moduls PayPal für den OXID eShop Version 6.2.

Systemvoraussetzungen
---------------------
Für die Verwendung des Moduls PayPal sind unten stehende Systemvoraussetzungen notwendig. Darüber hinaus funktioniert das Modul PayPal nur, wenn der OXID eShop für den SSL-Modus konfiguriert wurde.

* PHP Versionen 7.1 bis 7.4
* URL
* OpenSSL

.. |schritt| image:: media/icons/schritt.jpg
               :class: no-shadow

--------------------------------------------------

Neu-Installation
----------------
Wurde der Shop als OXID eShop Compilation aufgesetzt, ist die passende Version des Moduls PayPal bereits integriert. Sie muss lediglich im Administrationsbereich des Shops aktiviert und konfiguriert werden.

|schritt| Modul PayPal installieren
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Für den Fall, dass das Modul manuell in den Shop integriert werden muss, wird das Modul aus dem Repository heruntergeladen und installiert. Dazu wird per Konsole folgendes Composer-Kommando im Hauptverzeichnis des Shops ausgeführt:

.. code:: bash

  composer require --update-no-dev oxid-esales/paypal-module:^6.0.0

|schritt| Berechtigung für das Logging setzen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Ändern Sie die Schreibrechte von :file:`/modules/oe/oepaypal/logs`. Geben Sie volle Schreibrechte für Besitzer und Gruppe (755 oder 777).

|schritt| Modul PayPal aktivieren
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Das Modul PayPal muss im Shop aktiviert werden. In der Registerkarte :guilabel:`Stamm` des Moduls drücken Sie auf die Schaltfläche :guilabel:`Aktivieren`.

|schritt| Temporäre Dateien löschen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Löschen Sie alle Dateien und Ordner außer der :file:`.htaccess` aus dem Verzeichnis :file:`/tmp` des Shops.

--------------------------------------------------

Update-Installation
-------------------

|schritt| Moduldateien löschen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Falls Sie das Modul PayPal separat aktualisieren müssen, löschen Sie alle Dateien und Verzeichnisse in :file:`/source/modules/oe/oepaypal`.

|schritt| Modul PayPal installieren
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Installieren Sie das aktuelle Modul mit folgendem Composer-Kommando, welches Sie per Konsole im Hauptverzeichnis des Shops ausführen:

.. code:: bash

  composer require --update-no-dev  oxid-esales/paypal-module:^6.0.0

|schritt| Update abschließen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Schließen Sie das Update ab, indem Sie - wie bei Neu-Installation beschrieben - die Berechtigung für das Logging setzen, das Modul im Administrationsbereich aktivieren und die temporären Dateien des Shop löschen.


.. Intern: oxdaab, Status: