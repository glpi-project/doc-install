Update
======

.. note::

   As for every update process, you have to backup some data before processing any upgrade:

   * **backup your database**;
   * backup your `config` directory, especially for your GLPI key file (`config/glpi.key` or `config/glpicrypt.key`) which is randomly generated;
   * backup your `files` directory, it contains users and plugins generated files, like uploaded documents;
   * backup your `marketplace` and `plugins` directory.

Here are the steps to update GLPI:

* Download latest GLPI version.
* Ensure the target directory is empty and extract files there.
* Restore the previously backed up `config`, `files`, `marketplace` and `plugins` directory.
* Then open the GLPI instance URI in your browser, or (recommended) use the `php bin/console db:update` :doc:`command line tool <command-line>`.

.. warning::

    As soon as a new version of GLPI files is detected, you will not be able to use the application until the update process has been done.

.. warning::

    You should not try to restore a database backup on a non empty database (say, a database that has been partially migrated for any reason).

    Make sure your database is empty before restoring your backup and try to update, and repeat on fail.

.. note::

    Update process will automatically disable your plugins.

.. note::

   Since GLPI 10.0.1, you can use the `php bin/console db:check` :doc:`command line tool <command-line>` before executing the update command.
   This will allow you to check the integrity of your database, and to identify changes to your database that could compromise the update.
