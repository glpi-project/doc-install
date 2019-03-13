Timezones
=========

In order to get timezones working on a MariaDB/MySQL instance, you will have to initialize Timezones data and grant GLPI database user read ACL on their table.

.. warning::

   Enabling timezone support on your MySQL instance may affect other database in the same instance; be carefull!

Non windows users
-----------------

On most systems, you'll have to initialize timezones data from your system's timezones:

.. code-block:: bash

   mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql -p -u root mysql

You may want to check `MariaDB documentation about mysql_tzinfo_to_sql <https://mariadb.com/kb/en/library/mysql_tzinfo_to_sql/>`_ and your system documentation do know where data are stored (if not in ``/usr/share/zoneinfo``).

Do not forget to restart the database server once command is successfull.

Windows users
-------------

Windows does not provide timezones informations, you'll have to download and intialize data yourself.

See `MariaDB documentation about timezones <https://mariadb.com/kb/en/library/time-zones/#mysql-time-zone-tables>`_.

Grant access
------------

.. warning::

   Be carefull not to give your GLPI database user too large access. System tables should **never** grant access to app users.

In order to list possible timezones, your GLPI database user must have read access on ``mysql.time_zone_name`` table.
Assuming your user is ``glpi@localhost``, you should run something like:

.. code-block:: sql

   GRANT SELECT ON `mysql`.`time_zone_name` TO 'glpi'@'localhost';
   FLUSH PRIVILEGES;
