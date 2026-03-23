Update
======

Before starting
---------------

1. Identify your GLPI settings.

You will need to identify the database being used by GLPI, your GLPI installation's directory location, and the locations of GLPI's ``config``, ``files``, and plugin directories.

If you do not know where your GLPI installation is, you may start your search within the default hosting directory used by your web server, such as "/var/www" for Apache on Linux.

* By default, the ``config`` and ``files`` directories are in the root of your GLPI installation but they may be moved as explained in the :ref:`installation documentation <files_and_dirs_locations>`.
* The database information can be found within the ``config_db.php`` file in GLPI's ``config`` directory.
* The directories GLPI will look at for plugins are usually the ``marketplace`` and ``plugins`` directories in the root of your GLPI installation but they may be moved by setting the ``GLPI_PLUGIN_DIRECTORIES`` constant in a ``config/local_define.php`` file or ``inc/downstream.php``.

At this point, you should also identify if any updates are required for GLPI's :doc:`prerequisites <prerequisites.rst>` such as the database server, PHP version, etc.

2. Back up user data

Using a tool appropriate for your database server, create a backup of the GLPI database.
Also make a backup of your GLPI installation's directory.
Then, make copies of the files and directories (as previously identified) containing user data. If these are all still located in the default locations inside the GLPI installation's folder, you do not need to make separate copies of them.

* GLPI's config directory, especially the ``glpi.key``, ``glpicrypt.key``, ``oauth.pem``, and ``oauth.pub`` files within it.
* GLPI's file directory which contains uploaded documents, certain plugin data, logs, custom palettes and more.
* All plugin directories.
* The ``inc/downstream.php`` file if it exists.

3. Install the new version

    .. warning::

        As soon as a new version of GLPI files is detected, you will not be able to use the application until the update process has been done.

    .. warning::

        You should not try to restore a database backup on a non empty database (say, a database that has been partially migrated for any reason).

        Make sure your database is empty before restoring your backup and try to update, and repeat on fail.

    .. note::

        You can use the ``php bin/console db:check`` :ref:`command line tool <cdline_dbcheck>` before updating to identify any inconsistencies in your database's schema and the expected one that may of been caused by past partially-failed updates.
        These inconsistencies could interfere with the GLPI update process and produce unpredictable results.

    1. Update GLPI's prerequisites as needed or desired.
    2. Move or delete the existing GLPI installation's directory. This is important as copying the new GLPI release over top an existing installation can cause application errors.
    3. Download the "glpi-<version>.tgz" from the latest GLPI release on the `GLPI GitHub releases page <https://github.com/glpi-project/glpi/releases>`_. GLPI 11 contains the necessary migrations for version 0.85 and greater, so it is possible to skip even major versions entirely if necessary.
    4. Extract the downloaded release archive to the GLPI installation's original location, renaming the "glpi" folder as needed for it to match the original GLPI installation's directory.
    5. Restore the previously backed up config, files, and plugin directories along with the ``inc/downstream.php`` file if it existed from the original GLPI.
    6. Open the GLPI installation's URL in your browser or use the ``php bin/console db:update`` :ref:`command line tool <cdline_update>` from inside the GLPI installation's directory to initiate the update. This process may take some time.


    .. note::

        Before GLPI 11.0, all the plugins were disabled during the update process.
        
        Since GLPI 11.0, as soon as a new version of GLPI files is detected, the plugins execution is suspended.
        Their execution will be resumed after the update only in case of a minor/bugfixes update.
        Otherwise, you will have to do it manually using the ``php bin/console plugin:resume_execution`` :ref:`command line tool <_cdline_suspend_resume_plugins>`
        or from the plugins administration page.