Command line tools
==================

Since GLPI 9.2.2, command line tools are provided as supported scripts and are available from the ``scripts`` directory of the archive. On previous versions, those scripts were present in the ``tools`` directory that is not official and therefore not in the release archive.

Since GLPI 9.4.0, command line tools are being centralized in a console application (``bin/console``).
Calling ``php bin/console`` from GLPI directory displays the list of available commands.

.. note::

   If APCu is installed on your system, it may fail from command line since default configuration disables it from command-line. To change that, set ``apc.enable_cli`` to ``on`` in your APCu configuration file.

.. warning::

   When using cli tools, please check the system user you are currently logged in with, and permissions on files and directories. With a wrong user, logs, cache and other files may be created with rights that would not allow your webserver to read or write on thos files!

.. _cdline_options:

Console options
---------------

For every console command, following options are available:

 * ``--config-dir=CONFIG-DIR`` path of configuration directory to use, relative to current working directory (required only if a custom path is used)
 * ``-h``, ``--help`` displays command help
 * ``--lang=LANG`` output language code (default value is existing GLPI "language" configuration or "en_GB")
 * ``-n``, ``--no-interaction`` disable command interactive questions
 * ``--no-plugins`` disable GLPI plugins during command execution
 * ``-q``, ``--quiet`` disable command output
 * ``-v|vv|vvv``, ``--verbose=VERBOSE`` verbosity level: 1 for normal output, 2 for more verbose output and 3 for debug

.. _cdline_install:

Additional install and update tools
-----------------------------------

Check requirements
^^^^^^^^^^^^^^^^^^

Before installing or upgrading, requirements are automatically checked; but you can run them separately and see state for all of them using the ``glpi:system:check_requirements`` command.

Enable/Disable maintenance
^^^^^^^^^^^^^^^^^^^^^^^^^^^

GLPI provides a maintenance mode that can be activatd prior to an update, and deactivated after all has been checked.

Just use the ``glpi:maintenance:enable`` and ``glpi:maintenance:disable`` commands.

Install
-------

The ``php bin/console db:install`` has been made to install GLPI database in CLI mode.

Possible options for this command are:

 * ``-r``, ``--reconfigure`` to enable overriding of any existing DB configuration file
 * ``-f``, ``--force`` to force execution of installation even if database is not empty
 * ``-L``, ``--default-language=DEFAULT_LANGUAGE`` default language of GLPI (`en_GB` per default)
 * ``-H``, ``--db-host=DB_HOST`` host name (`localhost` per default)
 * ``-P``, ``--db-port=DB_PORT`` database port (default MySQL port if option is not defined)
 * ``-d``, ``--db-name=DB_NAME`` database name
 * ``-u``, ``--db-user=DB_USER`` database user name
 * ``-p``, ``--db-password=DB_PASSWORD`` database user's pasword (use it without value to be prompted for password)

If mandatory options are not specified in the command call, the console will ask for them.

Database connection parameters may be ommited if a configuration file already exists.

See also :ref:`console options <cdline_options>`.

Database connection configuration
---------------------------------

   .. versionadded:: 9.5.0

The ``php bin/console db:configure`` has been made to define database connection parameters in CLI mode.

Possible options for this command are:

 * ``-r``, ``--reconfigure`` to enable overriding of any existing DB configuration file
 * ``-H``, ``--db-host=DB_HOST`` host name (`localhost` per default)
 * ``-P``, ``--db-port=DB_PORT`` database port (default MySQL port if option is not defined)
 * ``-d``, ``--db-name=DB_NAME`` database name
 * ``-u``, ``--db-user=DB_USER`` database user name
 * ``-p``, ``--db-password=DB_PASSWORD`` database user's pasword (use it without value to be prompted for password)

If mandatory options are not specified in the command call, the console will ask for them.

See also :ref:`console options <cdline_options>`.

.. _cdline_update:

Update
------

The ``php bin/console db:update`` has been made to update GLPI database in CLI mode from a previously installed version.

There is no required arguments, just run the command so it updates your database automatically.

.. warning::

   Do not forget to backup your database before any update try!

Possible options for this command are:

 * ``-u``, ``--allow-unstable`` allow update to an unstable version (use it with cautions)
 * ``-f``, ``--force`` force execution of update from v-1 version of GLPI even if schema did not changed

See also :ref:`console options <cdline_options>`.

Various tools
--------------

Database schema check
^^^^^^^^^^^^^^^^^^^^^
The ``php bin/console db:check`` command can be used to check if your database schema differs from expected one.

If you have any diff, output will looks like :

.. code-block:: none

    $ php bin/console glpi:database:check
    Table schema differs for table "glpi_rulecriterias".
    --- Original
    +++ New
    @@ @@
     create table `glpi_rulecriterias` (
       `id` int(11) not null auto_increment
       `rules_id` int(11) not null default '0'
       `criteria` varchar(255) default null
       `condition` int(11) not null default '0'
    -  `pattern` text default null
    +  `pattern` text
       primary key (`id`)

LDAP synchonization
^^^^^^^^^^^^^^^^^^^

The ``bin/console glpi:ldap:synchronize_users`` command can be used to synchronize users against LDAP server informations.

Possible options for this command are:

 * ``-c``, ``--only-create-new`` only create new users
 * ``-u``, ``--only-update-existing`` only update existing users
 * ``-s``, ``--ldap-server-id[=LDAP-SERVER-ID] `` synchronize only users attached to this LDAP server (multiple values allowed)
 * ``-f``, ``--ldap-filter[=LDAP-FILTER]`` filter to apply on LDAP search
 * ``--begin-date[=BEGIN-DATE]`` begin date to apply in "modifyTimestamp" filter
 * ``--end-date[=END-DATE]`` end date to apply in "modifyTimestamp" filter
 * ``-d``, ``--deleted-user-strategy[=DELETED-USER-STRATEGY] force strategy used for deleted users:
 
    * 0: Preserve
    * 1: Put in trashbin
    * 2: Withdraw dynamic authorizations and groups
    * 3: Disable
    * 4: Disable + Withdraw dynamic authorizations and groups

See http://php.net/manual/en/datetime.formats.php for supported date formats in ``--begin-date`` and ``--end-date`` options.

See also :ref:`console options <cdline_options>`.

Task unlock
^^^^^^^^^^^

The ``php bin/console task:unlock`` command can be used to unlock stucked cron tasks.

.. warning::

   Keep in mind that no task should be stucked except in case of a bug or a system failure (database failure during cron execution for example).

Possible options for this command are:

 * ``-a``, ``--all`` unlock all tasks
 * ``-c``, ``--cycle[=CYCLE]`` execution time (in cycles) from which the task is considered as stuck (delay = task frequency * cycle)
 * ``-d``, ``--delay[=DELAY]`` execution time (in seconds) from which the task is considered as stuck (default: 1800)
 * ``-t``, ``--task[=TASK]`` ``itemtype::name`` of task to unlock (e.g: ``MailCollector::mailgate``)

See also :ref:`console options <cdline_options>`.

Plugins tools
-------------

    .. versionadded:: 9.5

Some command line tolls are also available to manage plugins from command line:

 * ``glpi:plugin:install``
 * ``glpi:plugin:activate``
 * ``glpi:plugin:deactivate``

In order to install ``MyGreatPlugin``; you should end with something like:

.. ::

   $ ./bin/console glpi:plugin:install MyGreatPlugin
   $ ./bin/console glpi:plugin:activate MyGreatPlugin

Each of those plugin commands can take a plugin name as argument, or the ``--all`` flag to be ran on all plugins.

Migration tools
---------------

From MyISAM to InnoDB
^^^^^^^^^^^^^^^^^^^^^

   .. versionadded:: 9.3.0

Since version 9.3.0, GLPI uses the ``InnoDB`` engine instead of previously used ``MyISAM`` engine.

The ``php bin/console glpi:migration:myisam_to_innodb`` command can be used to migrate exiting tables to ``InnoDB`` engine.

Missing timestamps builder
^^^^^^^^^^^^^^^^^^^^^^^^^^

   .. versionadded:: 9.1.0

Prior to GLPI 9.1.0, fields corresponding to creation and modification dates were not existing.

The ``php bin/console glpi:migration:build_missing_timestamps`` command can be used to rebuild missing values using available logs.

Use timestamp data type
^^^^^^^^^^^^^^^^^^^^^^^

   .. versionadded:: 9.5.0

Many date fields were using the ``DATETIME`` type, but this does not allow to rely on timezones. Timezone support requires all fields to use ``TIMESTAMP`` data type, but this query can be very long and therefore is not included in the standard update process.

Using the ``glpi:migration:timestamps`` command will change those fields to the correct data type.

.. warning::

   Ensure to backup your database before!
