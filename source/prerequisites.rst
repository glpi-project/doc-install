Prerequisites
=============

GLPI is a Web application that will need:

* a webserver;
* PHP;
* a database.

Web server
----------

GLPI requires a web server that supports PHP, like:

* `Apache 2 (or more recent) <http://httpd.apache.org>`_;
* `Nginx <http://nginx.org/>`_;
* `Microsoft IIS <http://www.iis.net>`_.

PHP
---

As of 9.2 release, GLPI requires `PHP <http://php.net>`_ 5.6 or more recent.

.. note::

   We recommand to use the most recent stable PHP release for better performances.

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
* ``xml``.

Optional extensions
^^^^^^^^^^^^^^^^^^^

.. note::

   Even if those extensions are not mandatory, we advise you to install them anyways.

Following PHP extensions are required for some extra features of GLPI:

* ``cli``: to use PHP from command line (scripts, automatic actions, and so on);
* ``domxml``: used for CAS authentication;
* ``imap``: used for mail collector ou user authentication;
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
    register_globals = off ;    // not mandatory but recommended
    magic_quotes_sybase = off ;
    session.auto_start = off ;
    session.use_trans_sid = 0 ; // not mandatory but recommended

Database
--------

.. warning::

   Currently, only `MySQL <https://dev.mysql.com>`_ (5.6 minimum) and `MariaDB <https://mariadb.com>`_ (10.0 minimum) database servers are supported by GLPI.

In order to work, GLPI requires a database server.
