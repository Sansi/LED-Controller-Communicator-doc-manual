# Installation

StarRiver Server has two modules: database system and StarRiver communication application.

## Prerequisites

StarRiver Server relies on .Net Framework 4.0. Please install the framework at the very beginning to prevent possible issues during installation of other modules.

The installer can be found at [Microsoft's webpage](https://www.microsoft.com/en-US/download/details.aspx?id=17718). A copy is provided in the StarRiver installation disk.

## Database Installation

StarRiver works with MySQL or its compatible replacements such as MariaDB. The following guide is based on MySQL Server 5.6 as an example.

First, download the installer from the [MySQL website](http://dev.mysql.com/downloads/mysql/). The MySQL Installer packaged for Windows platform is recommended. [MySQL Installer 5.6.19](http://dev.mysql.com/downloads/windows/installer/5.6.html) is adopted here.

Here are the installation steps.

1. The installer shows a welcome page initially. Select "Install MySQL Products".
   ![](img/mysql_1.png)
2. Now the installer checks for updates. If there is no Internet connection, tick "Skip the check for updates" and continue.
   ![](img/mysql_2.png)
3. Select "Server only" as the setup type and specify the data path. Here we use `C:\MySQL_Data` as an example.
   ![](img/mysql_3.png)
4. The installer will check for addition requirements to be installed. Let it finish.
   ![](img/mysql_4.png)
5. Click "Execute" to install MySQL Server.
   ![](img/mysql_5.png)
6. Installation finishes. Click "Next" to start configuration.
   ![](img/mysql_6.png)
7. Select "Server Machine" as the config type and continue.
   ![](img/mysql_7.png)
8. Now we create uses. Enter a password for the `root` user. **Please remember the password set here.** Click "Add User" to create a dedicated user for StarRiver. Use `sansi` as the username and `starriver` as the password. **If you customize the username or password, make sure you remember what is set here as the information will be used later.**
   ![](img/mysql_8.png)
9. MySQL Server starts as a Windows service. Just accept the default configuration and continue.
   ![](img/mysql_9.png)
10. Continue after the configuration procedure finishes.
   ![](img/mysql_10.png)
11. Installation complete.
   ![](img/mysql_11.png)

Then install [MySQL Workbench](http://dev.mysql.com/downloads/workbench/), a GUI management tool. MySQL Workbench 6.1.7 is adopted in this example. Accept all defaults during installation.

Finally, create the database structure StarRiver uses.

1. Open MySQL Workbench. Click the "+" button next to "MySQL Connection" to create a new MySQL connection.
   ![](img/db_init_1.png)
2. Enter "StarRiver" for connection name, "sansi" for username. Click "Store in Vault" and enter "starriver" for password. (If you use a custom database user in step 8 during MySQL installation, use that instead.)
   ![](img/db_init_2.png)
   ![](img/db_init_3.png)
   Click "Test Connection". If there is an error, check username and password entered.
   ![](img/db_init_4.png)
3. Click the newly created connection to connect to the database.
   ![](img/db_init_5.png)
4. Select "File->Open SQL Script".
   ![](img/db_init_6.png)
5. Open `init_db.sql`. Click the execute button highlighted with a red box below.
   ![](img/db_init_7.png)
6. The script finishes execution with no errors.
   ![](img/db_init_8.png)
7. Execute `events.sql` the same way.
   ![](img/db_init_9.png)
8. Check "Output" panel for errors.
   ![](img/db_init_10.png)
9. Click the refresh button in "Schemas" panel. Verify the a database named `led_control` has been setup.
   ![](img/db_init_12.png)

Some extra configuration is necessary for the database server by editing `my.ini` in MySQL data path (which is `C:\MySQL_Data` in this example.)

![](img/my_ini.png)

Add the following lines under `[mysqld]` section.

```
event-scheduler=on
lower_case_table_names=2

collation-server=utf8_general_ci
init-connect='SET collation_connection=utf8_unicode_ci'
init-connect='SET NAMES utf8'
skip-character-set-client-handshake
```

**Note that all quote marks are plain `'`.**

The new configuration will take effect after rebooting the system or restarting MySQL service.


## Install StarRiver Communication Application

Run `setup.exe` and it just works.

![](img/setup.png)

If you use custom user to connect to the database, you need to edit `config.ini` in StarRiver installation path.

![](img/config.png)

Change the values to allow StarRiver to connect to the database successfully.
