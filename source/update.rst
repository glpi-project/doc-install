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

.. warning::

   .. versionchanged: 10.0.0

   From the beginning, it was possible to update to any GLPI version from any older version. But this requires a lot of work to maintain so much very, very old code that can't even be tested...

   We've decided to support migrations from GLPI 0.80 only. If you want to update from an older version, you'll ineed to proceed in two steps, first updating to 9.4.x, then to 10.x.
