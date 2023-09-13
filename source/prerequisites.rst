Prerequisites
=============

GLPI is a Web application that will need:

* a webserver;
* PHP;
* a database.

.. _webserver_configuration:

Web server
----------

GLPI requires a web server that supports PHP, like:

* `Apache 2 (or more recent) <http://httpd.apache.org>`_;
* `Nginx <http://nginx.org/>`_;
* `lighttpd <https://www.lighttpd.net>`_;
* `Microsoft IIS <http://www.iis.net>`_.

Apache configuration
^^^^^^^^^^^^^^^^^^^^

Here is a virtual host configuration example for ``Apache 2`` web server.

.. warning::
   The following configuration is only suitable for GLPI version 10.0.7 or later.

.. code-block:: apache

    <VirtualHost *:80>
        ServerName glpi.localhost

        DocumentRoot /var/www/glpi/public

        # If you want to place GLPI in a subfolder of your site (e.g. your virtual host is serving multiple applications),
        # you can use an Alias directive. If you do this, the DocumentRoot directive MUST NOT target the GLPI directory itself.
        # Alias "/glpi" "/var/www/glpi/public"

        <Directory /var/www/glpi/public>
            Require all granted

            RewriteEngine On

            # Redirect all requests to GLPI router, unless file exists.
            RewriteCond %{REQUEST_FILENAME} !-f
            RewriteRule ^(.*)$ index.php [QSA,L]
        </Directory>
    </VirtualHost>

.. note::
   If you cannot change the ``Apache`` configuration (e.g. you are using a shared hosting), you can use a ``.htaccess`` file.

.. code-block:: apache

    # /var/www/glpi/.htaccess
    RewriteBase /
    RewriteEngine On
    RewriteCond %{REQUEST_URI} !^/public
    RewriteRule ^(.*)$ public/index.php [QSA,L]

Nginx configuration
^^^^^^^^^^^^^^^^^^^

Here is a configuration example for ``Nginx`` web server using ``php-fpm``.

.. warning::
   The following configuration is only suitable for GLPI version 10.0.7 or later.

.. code-block:: nginx

    server {
        listen 80;
        listen [::]:80;

        server_name glpi.localhost;

        root /var/www/glpi/public;

        location / {
            try_files $uri /index.php$is_args$args;
        }

        location ~ ^/index\.php$ {
            # the following line needs to be adapted, as it changes depending on OS distributions and PHP versions
            fastcgi_pass unix:/run/php/php-fpm.sock;

            fastcgi_split_path_info ^(.+\.php)(/.*)$;
            include fastcgi_params;

            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        }
    }

lighttpd configuration
^^^^^^^^^^^^^^^^^^^^^^

Here is a virtual host configuration example for ``lighttpd`` web server.

.. warning::
   The following configuration is only suitable for GLPI version 10.0.7 or later.

.. code-block:: lighttpd

    $HTTP["host"] =~ "glpi.localhost" {
        server.document-root = "/var/www/glpi/public/"

        url.rewrite-if-not-file = ( "" => "/index.php${url.path}${qsa}" )
    }


IIS configuration
^^^^^^^^^^^^^^^^^

Here is a ``web.config`` configuration file example for ``Microsoft IIS``.
The physical path of GLPI web site must point to the ``public`` directory of GLPI (e.g. ``D:\glpi\public``), and the ``web.config`` file must be placed inside this directory.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
       <system.webServer>
           <rewrite>
               <rules>
                   <rule name="Rewrite to GLPI" stopProcessing="true">
                       <match url="^(.*)$" />
                       <conditions>
                           <add input="{REQUEST_FILENAME}" matchType="IsFile" ignoreCase="false" negate="true" />
                       </conditions>
                       <action type="Rewrite" url="index.php" appendQueryString="true" />
                   </rule>
             </rules>
           </rewrite>
       </system.webServer>
   </configuration>

.. warning::
   The `URL Rewrite <https://www.iis.net/downloads/microsoft/url-rewrite>`_ module is required.

PHP
---

.. list-table:: PHP Compatibility Matrix
   :header-rows: 1

   * - GLPI Version
     - Minimum PHP
     - Maximum PHP
   * - 9.5.X
     - 7.2
     - 8.0
   * - 10.0.X
     - 7.4
     - 8.2

.. note::

   We recommend to use the newest supported PHP release for better performance.

Mandatory extensions
^^^^^^^^^^^^^^^^^^^^

Following PHP extensions are required for the app to work properly:

* ``dom``, ``fileinfo``, ``filter``, ``libxml``, ``json``, ``simplexml``, ``xmlreader``, ``xmlwriter``: these PHP extensions are enable by default and are used for various operations;
* ``curl``: used for remote access to resources (inventory agent requests, marketplace, RSS feeds, ...);
* ``gd``: used for images handling;
* ``intl``: used for internationalization;
* ``mysqli``: used for database connection;
* ``session``: used for sessions support;
* ``zlib``: used for handling of compressed communication with inventory agents, installation of gzip packages from marketplace and PDF generation.

Optional extensions
^^^^^^^^^^^^^^^^^^^

.. note::

   Even if those extensions are not mandatory, we advise you to install them anyways.

Following PHP extensions are required for some extra features of GLPI:

* ``bz2``, ``Phar``, ``zip``: enable support of most common packages formats in marketplace;
* ``exif``: enhance security on images validation;
* ``ldap``:  enable usage of authentication through remote LDAP server;
* ``openssl``: enable email sending using SSL/TLS;
* ``Zend OPcache``: enhance PHP engine performances.

Database
--------

.. warning::

   Currently, only `MySQL <https://dev.mysql.com>`_ (5.7 minimum) and `MariaDB <https://mariadb.com>`_ (10.2 minimum) database servers are supported by GLPI.

In order to work, GLPI requires a database server.
