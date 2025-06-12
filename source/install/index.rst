Install GLPI
============

Proceed as follow:

#. :ref:`Configure your webserver <webserver_configuration>`,
#. Choose a version,
#. Download the archive,
#. Install :)

Choose a version
----------------

.. note::

   It is hightly recommended you choose the latest stable release for a production usage.

GLPI follows a semantic versioning scheme, on 3 digits. The first one is the major release, the second the minor and the third the fix release.

Major releases may come with important incompatibilities as well as new features; minor versions may bring new features as well, but stay perfectly compatible inside a major version.

Fixes releases will only fix reported issues without adding anything new.

Download
--------

.. warning::

   On GitHub, there are always two archives named *Source code* which should not be used.

Go to the *download* section of `the GLPI website <http://glpi-project.org>`_ (or get archive directly from `Github release <https://github.com/glpi-project/glpi/releases>`_) and choose the ``glpi-{version}.tgz`` archive.

Installation
------------


GLPI installation itself is composed of three steps:

#. Uncompress the archive in your website;
#. Give your webserver write access to the ``files`` and ``config`` directories;
#. Launch :doc:`installation wizard <wizard>` (or use the :ref:`command line installation script <cdline_install>`).

Once these three steps have been completed the application is ready to be used.

If you need to set advanced configuration, like SSL connection parameters, please refer to :doc:`advanced configuration <advanced-configuration>`.

Files and directories locations
-------------------------------

Like many other web applications, GLPI can be installed by just copying the whole directory to any web server. However, this may be less secure.

.. warning::

   Every file accessible directly from a web server must be considered unsafe!

GLPI stores some data in the ``files`` directory, the database access configuration is stored in the ``config`` directory, etc. Even if GLPI provides some ways to prevent files from being accessed by the webserver directly, best practise is to store data outside of the web root. That way, sensitive files cannot be accessed directly from the web server.

There are a few configuration directives you may use to achieve that (directives that are used in provided downstream packages):

* ``GLPI_CONFIG_DIR``: set path to the configuration directory;
* ``GLPI_VAR_DIR`` : set path to the ``files`` directory;
* ``GLPI_LOG_DIR`` : set path to logs files.

.. note::

   There are many other configuration directives available, the ones we talked about are the main to take into account for a more secure installation.

Directories choice is entirely up to you; the following example will follow the `FHS <http://www.pathname.com/fhs/>`_ recommendations.

Our GLPI instance will be installed in ``/var/www/glpi``, a specific virtual host in the web server configuration will reflect this path.

GLPI configuration will be stored in ``/etc/glpi``, just copy the contents of the ``config`` directory to this place. GLPI requires read rights on this directory to work; and write rights during the installation process.

GLPI data will be stored in ``/var/lib/glpi``, just copy the contents of the ``files`` directory to this place. GLPI requires read and write rights on this directory.

GLPI logs files will be stored in ``/var/log/glpi``, there is nothing to copy here, just create the directory. GLPI requires read and write access on this directory.

Following this instructions, we'll create a ``inc/downstream.php`` file into GLPI directory with the following contents:

.. code-block:: php

   <?php
   define('GLPI_CONFIG_DIR', '/etc/glpi/');

   if (file_exists(GLPI_CONFIG_DIR . '/local_define.php')) {
      require_once GLPI_CONFIG_DIR . '/local_define.php';
   }


.. warning::

   GLPI packages will certainly provide a ``inc/downstream.php`` file. This one must not be edited!

   GLPI looks for a `local_define.php` file in its own `config` directory. If you want to use one from new config directory, you have to load it.

Then, create a file in ``/etc/glpi/local_define.php`` with the following contents:

.. code-block:: php

   <?php
   define('GLPI_VAR_DIR', '/var/lib/glpi');
   define('GLPI_LOG_DIR', '/var/log/glpi');

.. note::

   .. versionadded:: 9.2.2

   For GLPI prior to 9.2.2, the ``GLPI_VAR_DIR`` constant did not exist and it was required to set all paths separately:

   .. code-block:: php

      <?php
      define('GLPI_VAR_DIR', '/var/lib/glpi');
      define('GLPI_DOC_DIR',        GLPI_VAR_DIR);
      define('GLPI_CRON_DIR',       GLPI_VAR_DIR . '/_cron');
      define('GLPI_DUMP_DIR',       GLPI_VAR_DIR . '/_dumps');
      define('GLPI_GRAPH_DIR',      GLPI_VAR_DIR . '/_graphs');
      define('GLPI_LOCK_DIR',       GLPI_VAR_DIR . '/_lock');
      define('GLPI_PICTURE_DIR',    GLPI_VAR_DIR . '/_pictures');
      define('GLPI_PLUGIN_DOC_DIR', GLPI_VAR_DIR . '/_plugins');
      define('GLPI_RSS_DIR',        GLPI_VAR_DIR . '/_rss');
      define('GLPI_SESSION_DIR',    GLPI_VAR_DIR . '/_sessions');
      define('GLPI_TMP_DIR',        GLPI_VAR_DIR . '/_tmp');
      define('GLPI_UPLOAD_DIR',     GLPI_VAR_DIR . '/_uploads');
      define('GLPI_CACHE_DIR',      GLPI_VAR_DIR . '/_cache');

      define('GLPI_LOG_DIR', '/var/log/glpi');

      Of course, it is always possible to redefine any of those paths if needed.

Post installation
-----------------

Once GLPI has been installed, you're almost done.

An extra step would be to secure installation directory. As an example, you can consider adding the following to your Apache virtual host configuration (or in the ``glpi/install/.htaccess`` file):

.. code-block:: apache

    <IfModule mod_authz_core.c>
        Require local
    </IfModule>
    <IfModule !mod_authz_core.c>
        order deny, allow
        deny from all
        allow from 127.0.0.1
        allow from ::1
    </IfModule>
    ErrorDocument 403 "<p><b>Restricted area.</b><br />Only local access allowed.<br />Check your configuration or contact your administrator.</p>"

With this example, the `install` directory access will be limited to localhost only and will display an error message otherwise. Of course, you may have to adapt this to your needs; refer to your web server's documentation.
