
A common query we get from our users is how to open a Windows database in the Weka Explorer. This page is intended as a guide to help you achieve this. It is a complicated process and we cannot guarantee that it will work for you. The process described makes use of the JDBC-ODBC bridge that is part of Sun's JRE/JDK 1.3 (and higher).

The following instructions are for Windows 2000. Under other Windows versions there may be slight differences.

# Step 1: Create a User DSN

* Go to the **Control Panel**
* Choose **Adminstrative Tools**
* Choose **Data Sources (ODBC)**
* At the **User DSN** tab, choose **Add...**
* Choose database
	* Microsoft Access
		* *Note:* Make sure your database is not open in another application before following the steps below.
		* Choose the **Microsoft Access** driver and click **Finish**
		* Give the source a name by typing it into the **Data Source Name** field
		* In the **Database** section, choose **Select...**
		* Browse to find your database file, select it and click **OK**
		* Click **OK** to finalize your DSN
	* Microsoft SQL Server 2000 (Desktop Engine)
		* Choose the **SQL Server** driver and click **Finish**
		* Give the source a name by typing it into the **Name** field
		* Add a description for this source in the **Description** field
		* Select the server you're connecting to from the **Server** combobox
		* For the verification of the authenticity of the login ID choose **With SQL Server...**
		* Check **Connect to SQL Server to obtain default settings...** and supply the user ID and password with which you installed the Desktop Engine
		* Just click on **Next** until it changes into **Finish** and click this, too
		* For testing purposes, click on **Test Data Source...** - the result should be *TESTS COMPLETED SUCCESSFULLY!*
		* Click on **OK**
	* MySQL
		* Choose the **MySQL ODBC** driver and click **Finish**
		* Give the source a name by typing it into the **Data Source Name** field
		* Add a description for this source in the **Description** field
		* Specify the server you're connecting to in **Server**
		* Fill in the user to use for connecting to the database in the **User** field, the same for the password
		* Choose the database for this DSN from the **Database** combobox
		* Click on **OK**
* Your DSN should now be listed in the **User Data Sources** list

# Step 2: Set up the DatabaseUtils.props file

You will need to configure a file called `DatabaseUtils.props`. This file already exists under the path `weka/experiment/` in the `weka.jar` file (which is just a ZIP file) that is part of the Weka download. In this directory you will also find a sample file for ODBC connectivity, called `DatabaseUtils.props.odbc`, and one specifically for MS Access, called `DatabaseUtils.props.msaccess` (>3.4.14, >3.5.8, >3.6.0), also using ODBC. You should use one of the sample files as basis for your setup, since they already contain default values specific to ODBC access.

This file needs to be recognized when the Explorer starts. You can achieve this by making sure it is in the working directory or the home directory (if you are unsure what the terms *working directory* and *home directory* mean, see the *Notes* section). The easiest is probably the second alternative, as the setup will apply to all the Weka instances on your machine.

Just make sure that the file contains the following lines at least:

```ini
 jdbcDriver=sun.jdbc.odbc.JdbcOdbcDriver
 jdbcURL=jdbc:odbc:dbname
```
where *dbname* is the name you gave the user DSN. (This can also be changed once the Explorer is running.)

# Step 3: Open the database

## Book version

* Start up the Weka Explorer.
* Choose **Open DB...**
* Edit the **query** field to read `select * from tablename` where *tablename* is the name of the database table you want to read, or you could put a more complicated SQL query here instead.
* The **databaseURL** should read "jdbc:odbc:*dbname*" where *dbname* is the name you gave the user DSN.
* Click **OK** 

At this point the data should be read from the database.

## Stable 3.6 and developer version

* Start up the Weka Explorer.
* Choose **Open DB...**
* The **URL** should read "jdbc:odbc:*dbname*" where *dbname* is the name you gave the user DSN.
* Click **Connect**
* Enter a **Query**, e.g., "`select * from tablename`" where *tablename* is the name of the database table you want to read. Or you could put a more complicated SQL query here instead.
* Click **Execute**
* When you're satisfied with the returned data, click **OK** to load the data into the Preprocess panel.

# Notes

* **Working directory**
> The directory a process is started from. When you start Weka from the Windows Start Menu, then this directory would be Weka's installation directory (the `java` process is started from that directory).
* **Home directory**
> The directory that contains all the user's data. The exact location depends on the operating system and the version of the operating system. It is stored in the following environment variable:

	* Unix/Linux
>> `$HOME`
	* Windows
>> `%USERPROFILE%`
	* Cygwin
>> `$USERPROFILE`
> You should be able output the value in a command prompt/terminal with the `echo` command. E.g., for Windows this would be:

>> `echo %USERPROFILE%`

# See also
* [Databases](databases.md)
* [CLASSPATH](classpath.md)
* [DatabaseUtils.props](weka_experiment_database_utils.props.md)
