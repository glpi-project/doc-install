Update
======

.. note::

   As for every update process, you have to backup some data before processing any upgrade:

   * **backup your database**;
   * backup your files directory;
   * backup your configuration.

GLPI update process is automated. To start it, just go to your GLPI instance URI, or (recommended) use the :doc:`command line tools <command-line>`.

Once a new version will be installed; you will not be able to use the application until a migration has been done.

Please also note the update process will automatically disable your plugins.

.. warning::

    You should not try to restore a database backup on a non empty database (say, a database that has been partially migrated for any reason).

    Make sure your database is empty before restoring your backup and try to update, and repeat on fail.
