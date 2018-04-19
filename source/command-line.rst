Command line tools
==================

Since GLPI 9.2.2, command line tools are provided as supported scripts and are available from the ``scripts`` directory of the archive. On previous versions, those scripts were present in the ``tools`` directory that is not official and therefore not in the release archive.

.. _cdline_install:

Install
-------

A PHP command line installation script is provided in the GLPI archive (``scripts/cliinstall.php``).

You have to specify at least a database name and an user:

.. code-block:: bash

   # php scripts/cliinstall.php --db=dbglpi --user=userglpi

It is possible to specifiy some variables calling the script:

 * ``--host`` host name (`localhost` per default),
 * ``--db`` database name,
 * ``--user`` database user name,
 * ``--pass`` database user's pasword,
 * ``--lang`` language code to use (`fr_FR` as example). Will be set to `en_GB` per default,
 * ``--tests`` create tests configuration file,
 * ``--force`` do not check if GLPI is already installed and drop what would exists,
 * ``--help`` displays command help.

.. _cdline_update:

Update
------

An update script is provided as well (``scripts/cliupdate.php``).

There is no required arguments, just run the script so it updates your database automatically and disables plugin(s) before.

.. warning::

   Do not forget to backup your database before any update try!

Possible options for this command are:

 * ``--lang`` language code to use (`fr_FR` as example). Will be set to `en_GB` per default,
 * ``--help`` displays command help.
 * ``--config-dir`` set configuration file path to use,
 * ``--force`` force update, usefull when GLPI version does not change (mainly when working on it ;)),
 * ``--dev`` required to use a development version. Usei it with cautions...
