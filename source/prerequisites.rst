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
        # you can use an Alias directive:
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
   If you cannot change the ``Apache`` configuration (e.g. you are using a shared hosting), you can use a ``.htaccess`` file:

.. code-block:: apache
   # /var/www/glpi/.htaccess
   RewriteBase /
   RewriteEngine On
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

IIS configuration
^^^^^^^^^^^^^^^^^

Here is a configuration example for ``Microsoft IIS``.

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

PHP
---

.. list-table:: PHP Compatibility Matrix
   :header-rows: 1

   * - GLPI Version
     - Minimum PHP
     - Maximum PHP
   * - 9.4.X
     - 5.6
     - 7.4
   * - 9.5.X
     - 7.2
     - 8.0
   * - 10.0.X
     - 7.4
     - 8.1

.. note::

   We recommend to use the newest supported PHP release for better performance.

Mandatory extensions
^^^^^^^^^^^^^^^^^^^^

Following PHP extensions are required for the app to work properly:

* ``curl``: for CAS authentication, GLPI version check, Telemetry, ...;
* ``fileinfo``: to get extra informations on files;
* ``gd``: to generate images;
* ``json``: to get support for JSON data format;
* ``mbstring``:  to manage multi bytes characters;
* ``mysqli``: to connect and query the database;
* ``session``: to get user sessions support;
* ``zlib``: to get backup and restore database functions;
* ``simplexml``;
* ``xml``;
* ``intl``.

Optional extensions
^^^^^^^^^^^^^^^^^^^

.. note::

   Even if those extensions are not mandatory, we advise you to install them anyways.

Following PHP extensions are required for some extra features of GLPI:

* ``cli``: to use PHP from command line (scripts, automatic actions, and so on);
* ``domxml``: used for CAS authentication;
* ``ldap``:  use LDAP directory for authentication;
* ``openssl``: secured communications;
* ``xmlrpc``: used for XMLRPC API.
* ``APCu``: may be used for cache; among others (see `caching configuration (in french only) <http://glpi-user-documentation.readthedocs.io/fr/latest/advanced/cache.html>`_.

Configuration
^^^^^^^^^^^^^

PHP configuration file (``php.ini``) must be adapted to reflect following variables:

.. code-block:: ini

    memory_limit = 64M ;        // max memory limit
    file_uploads = on ;
    max_execution_time = 600 ;  // not mandatory but recommended
    session.auto_start = off ;
    session.use_trans_sid = 0 ; // not mandatory but recommended

Database
--------

.. warning::

   Currently, only `MySQL <https://dev.mysql.com>`_ (5.7 minimum) and `MariaDB <https://mariadb.com>`_ (10.2 minimum) database servers are supported by GLPI.

In order to work, GLPI requires a database server.
