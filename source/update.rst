Update
======

.. note::

   As for every update process, you have to backup some data before processing any upgrade:

   * **backup your database**;
   * backup your ``config`` directory, especially for your GLPI key files (``config/glpi.key``, ``config/glpicrypt.key``, ``config/oauth.pem``, ``config/oauth.pub``) which are randomly generated;
   * backup your ``files`` directory, it contains users and plugins generated files, like uploaded documents;
   * backup your ``marketplace`` and ``plugins`` directories.

Here are the steps to update GLPI:

* Download latest GLPI version.
* Ensure the target directory is empty and extract files there.
* Restore the previously backed up ``config``, ``files``, ``marketplace`` and ``plugins`` directories.
* Then open the GLPI instance URI in your browser, or (recommended) use the ``php bin/console db:update`` :ref:`command line tool <cdline_update>`.

.. warning::

    As soon as a new version of GLPI files is detected, you will not be able to use the application until the update process has been done.

.. warning::

    You should not try to restore a database backup on a non empty database (say, a database that has been partially migrated for any reason).

    Make sure your database is empty before restoring your backup and try to update, and repeat on fail.

.. note::

    Before GLPI 11.0, all the plugins were disabled during the ppdate process.
    
    Since GLPI 11.0, as soon as a new version of GLPI files is detected, the plugins execution is suspended.
    Their execution will be resumed after the update only in case of a minor/bugfixes update.
    Otherwise you will have to do it manually using the ``php bin/console plugin:resume_execution`` :ref:`command line tool <_cdline_suspend_resume_plugins>`
    or from the plugins administration page.

.. note::

   You can use the ``php bin/console db:check`` :ref:`command line tool <cdline_dbcheck>` before executing the update command.
   This will allow you to check the integrity of your database, and to identify changes to your database that could compromise the update.
