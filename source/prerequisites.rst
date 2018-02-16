Prerequisites
=============

GLPI uses following technologies:

* PHP as language;
* MySQL as database;
* HTML for displaying webpages;
* CSS for stylesheets;
* CSV, PDF et SLK for data exports;
* AJAX for UI dynamic elements;
* ...

Web server
----------

Web server characteristics to get GLPI running.

Web server
^^^^^^^^^^

GLPI requires a web server that supports PHP, like:

* `Apache 2 (or more recent) <http://httpd.apache.org>`_;
* `Nginx <http://nginx.org/>`_;
* `Microsoft IIS <http://www.iis.net>`_.

PHP
^^^

As of 9.2 release, GLPI requires `PHP <http://php.net>`_ 5.6 or more recent.

.. note::

   We recommand to use the most recent stable PHP release for better performances.

Mandatory extensions
++++++++++++++++++++

Following PHP extensions are required for the app to work properly:

* ``curl``: for CAS authentication, GLPI version check, Telemetry, ...;
* ``fileinfo``: to get extra informations on files;
* ``gd``: to generate images;
* ``json``: to get support for JSON data format;
* ``mbstring``:  to manage multi bytes characters;
* ``mysqli``: to connect and query the database liaison avec la base de données ;
* ``session``: to get user sessions support;
* Zlib: to get backup and restore database functions.

Optional extensions
+++++++++++++++++++

.. note::

   Even if those extensions are not mandatory, we advice you to install them anyways.

Following PHP extensions are required for some extra features of GLPI:

* ``cli``: to use PHP from command line (scripts, automatic actions, and so on);
* ``domxml``: used for CAS authentication;
* ``imap``: used for mail collector ou user authentication;
* ``ldap``:  use LDAP directory for authentication;
* ``openssl``: secured communications.

Configuration
+++++++++++++

PHP configuration file (``php.ini``) must be adapted to reflect following variables:

.. code-block:: ini

    memory_limit = 64M ;        // max memory limit
    file_uploads = on ;
    max_execution_time = 600 ;  // not mandatory but adviced
    register_globals = off ;    // not mandatory but adviced
    magic_quotes_sybase = off ;
    session.auto_start = off ;
    session.use_trans_sid = 0 ; // not mandatory but adviced

Database
--------

.. warning::

   Currently, only `MySQL <https://dev.mysql.com>`_ (5.1 minimum) and `MariaDB <https://mariadb.com>`_ database servers are supported by GLPI.

In order to work, GLPI requires a database server.

.. note::

   We recommand to use at least 5.5 version if you use MySQL for better performances.

Update
------

When updating your GLPI database, please make sure **you did a backup** before doing anything!

Also keep in mind that you should not try to restore a database backup on a non empty database (say, a database that has been partially migrated for any reason). Make sur your database is emm
 before restoring your backup and try to update.
