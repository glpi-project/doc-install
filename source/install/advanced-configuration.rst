Advanced configuration
======================

SSL connection to database
--------------------------

   .. versionadded:: 9.5.0

Once installation is done, you can update the ``config/config_db.php`` to define SSL connection parameters.
Available parameters corresponds to parameters used by `mysqli::ssl_set() <https://www.php.net/manual/en/mysqli.ssl-set.php>`:

* ``$dbssl`` defines if connection should use SSL (`false` per default)
* ``$dbsslkey`` path name to the key file (`null` per default)
* ``$dbsslcert`` path name to the certificate file (`null` per default)
* ``$dbsslca`` path name to the certificate authority file (`null` per default)
* ``$dbsslcapath`` pathname to a directory that contains trusted SSL CA certificates in PEM format (`null` per default)
* ``$dbsslcacipher`` list of allowable ciphers to use for SSL encryption (`null` per default)

.. warning::

   For now it is not possible to define SSL connection parameters prior or during the installation process.
   It has to be done once installation has been done.
