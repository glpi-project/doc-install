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

If you need to set advanced configuration, like SSL database connection parameters, please refer to :doc:`advanced configuration <advanced-configuration>`.

Files and directories locations
-------------------------------

As long as the web server is configured properly (see :ref:`web server configuration <webserver_configuration>`), sensible files should not be exposed publicly.

GLPI stores some data in the ``files`` directory, the database access configuration is stored in the ``config`` directory, etc. To ease the GLPI maintenance, the location of GLPI storage directories can be customized.

There are a few configuration directives you may use to achieve that:

* ``GLPI_CONFIG_DIR``: set path to the configuration directory;
* ``GLPI_VAR_DIR`` : set path to the ``files`` directory;
* ``GLPI_LOG_DIR`` : set path to logs files.

.. note::

   There are many other configuration directives available, the ones we talked about are the main to move all stored files outside the GLPI source code.

Directories choice is entirely up to you; the following example will follow the `FHS <http://www.pathname.com/fhs/>`_ recommendations.

Our GLPI instance will be installed in ``/var/www/glpi``, a specific virtual host in the web server configuration will reflect this path.

GLPI configuration will be stored in ``/etc/glpi``, just copy the contents of the ``config`` directory to this place. GLPI requires read rights on this directory to work; and write rights during the installation process.

GLPI data will be stored in ``/var/lib/glpi/files``, just copy the contents of the ``files`` directory to this place. GLPI requires read and write rights on this directory.

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
   define('GLPI_VAR_DIR', '/var/lib/glpi/files');
   define('GLPI_LOG_DIR', '/var/log/glpi');

.. note::

   ``GLPI_VAR_DIR`` permits to change the storage path of all the GLPI files, but you can adapt the storage path for each kind of files.

   .. code-block:: php

      <?php
      define('GLPI_VAR_DIR',        '/var/lib/glpi/files');
      define('GLPI_DOC_DIR',        GLPI_VAR_DIR);                  // Path for documents storage
      define('GLPI_CACHE_DIR',      GLPI_VAR_DIR . '/_cache');      // Path for cache storage
      define('GLPI_CRON_DIR',       GLPI_VAR_DIR . '/_cron');       // Path for cron storage
      define('GLPI_GRAPH_DIR',      GLPI_VAR_DIR . '/_graphs');     // Path for graph storage
      define('GLPI_LOCAL_I18N_DIR', GLPI_VAR_DIR . '/_locales');    // Path for local i18n files
      define('GLPI_LOCK_DIR',       GLPI_VAR_DIR . '/_lock');       // Path for lock files storage (used by cron)
      define('GLPI_LOG_DIR',        GLPI_VAR_DIR . '/_log');        // Path for log storage
      define('GLPI_PICTURE_DIR',    GLPI_VAR_DIR . '/_pictures');   // Path for picture storage
      define('GLPI_PLUGIN_DOC_DIR', GLPI_VAR_DIR . '/_plugins');    // Path for plugins documents storage
      define('GLPI_RSS_DIR',        GLPI_VAR_DIR . '/_rss');        // Path for RSS feeds storage
      define('GLPI_SESSION_DIR',    GLPI_VAR_DIR . '/_sessions');   // Path for sessions files storage
      define('GLPI_TMP_DIR',        GLPI_VAR_DIR . '/_tmp');        // Path for temporary files storage
      define('GLPI_UPLOAD_DIR',     GLPI_VAR_DIR . '/_uploads');    // Path for upload storage
      define('GLPI_INVENTORY_DIR',  GLPI_VAR_DIR . '/_inventories');// Path for inventory files storage
      define('GLPI_THEMES_DIR',     GLPI_VAR_DIR . '/_themes');     // Path for custom themes storage

Plugins files locations
^^^^^^^^^^^^^^^^^^^^^^^

   .. versionadded:: 11.0.0

Plugins files location can be configured using the ``GLPI_MARKETPLACE_DIR`` configuration directive.

To store the plugins in the ``/var/lib/glpi/plugins`` directory, just copy the contents of the ``marketplace`` and ``plugins`` directories to this place. GLPI requires read and write rights on this directory.

Then, in the ``/etc/glpi/local_define.php`` file, add the following contents:

.. code-block:: php

   define('GLPI_MARKETPLACE_DIR', '/var/lib/glpi/plugins');
