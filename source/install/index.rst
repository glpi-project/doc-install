Install GLPI
============

Preceed as follow:

#. Choose a version,
#. Download the archive,
#. Install :)

Choose a version
----------------

.. note::

   It is hightly recommended you choose the latest stable release for a production usage.

GLPI follows a semantic versionning scheme, on 3 digits. The first one is the majo release, the second the minor and the third the fix release.

Major releases may come with important incompatibilities as well as new features; minor versions may bring new features as well, but stay perfectly compatible inside a major version.

Fixes releases will only fix reported issues without adding anything new.

Download
--------

.. warning::

   On GitHub, there is always two archives named *Source code* which should not be used.

Go to the *download* section of `the GLPI website <http://glpi-project.org>`_ or get archive directly from `Github release <https://github.com/glpi-project/glpi/releases>`_.

Installation
------------

.. note::

   Packages may be availaible from your distribution (Red Hat, CentOS, Fedora, Ubuntu, ...) that you should use with your standard packages manager as usual.

GLPI installation itself is composed of three steps:

#. Uncompress archive in your website;
#. Give your webserver write access to ``files`` and ``config`` directories;
#. :doc:`launch installation wizard <wizard>` (or use :ref:`command line installation script <cdline_install>`).

Once three steps has been completed the app is ready to be used.

Files and directories emplacements
----------------------------------

As many other web applications, GLPI may be installed just by copying the whole directory to any web server. Anyways, this may be secure less.

.. warning::

   Every file accessible directly from a web server must be considered unsafe!

GLPI stores some data in the ``files`` directory, database access configuration is stored in the ``config`` directory, ... Even if GLPI provides some securities to prevent files to be accessed from a webserver directly, the best way to go is to store data outside of the web root. That way, sensible files cannot be accessed directly from the web server.

There are a few configuration directives you may use to achieve that (directives that are used in provided downstream packages):

* ``GLPI_CONFIG_DIR``: set path to the configuration directory;
* ``GLPI_VAR_DIR`` : set path to the ``files`` directory;
* ``GLPI_LOG_DIR`` : set path to logs files.

.. note::

   There ara many other configuration directives availaible, the ones we talked about are the main ones to take into account for a more secure installation.

Directories choice is entirely up to you; the following example will follow the `FHS <http://www.pathname.com/fhs/>`_ recommandations.

Our GLPI instance will be installed in ``/var/www/glpi`` directory, a specific virtual host in the web server configuration will reflect this path.

GLPI configuration will be stored in ``/etc/glpi``, just copy the ``config`` directory to this place. GLPI requires read rights on this directory to work; and write rights during the installation process.

GLPI data will be stored in ``/var/lib/glpi``, just copy the ``files`` directory to this place. GLPI requires read and write rights on this directory.

GLPI logs files will be stored in ``/var/log/glpi``, there is nothing to copye here, just create the directory. GLPI requires read and write access on this directory.

Following this instructions, we'll create a ``inc/downstream.php`` file into GLPI directory with the following contents:

.. code-block:: php

   <?php
   define('GLPI_CONFIG_DIR', '/etc/glpi/');

.. warning::

   GLPI packages will certainly provide a ``inc/downstream.php`` file. This one must not be edited!

Then, create a file in ``/etc/glpi/local_define.php`` with the following contents:

.. code-block:: php

   <?php
   define('GLPI_VAR_DIR', '/var/lib/glpi');
   define('GLPI_LOG_DIR', '/var/log/glpi');

.. note::

   .. versionadded:: 9.2.2

   For GLPI prior to 9.2.2, ``GLPI_VAR_DIR`` constant did not exists. It was required to set all paths separately:

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

      Of cource, it is always possible to redefine any of thos paths if needed.

.. note::

   GLPI configuration directory cannot be defined in the ``local_define.php`` file just because this one will be... in the configuration directory itself ;)
