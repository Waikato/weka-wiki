# CLASSPATH
See the [CLASSPATH](classpath.md) article for how to set up your CLASSPATH environment variable, in order to make the JDBC driver available for Weka.

# Configuration files
Thanks to JDBC it is easy to connect to Databases that provide a JDBC driver. Responsible for the setup is the following properties file, located in the `weka.experiment` package:

```
 DatabaseUtils.props
```

You can get this properties file from the `weka.jar` or `weka-src.jar` jar-archive, both part of a normal Weka release. If you open up one of those files, you'll find the properties file in the sub-folder `weka/experiment`.

Weka comes with example files for a wide range of databases:

* `DatabaseUtils.props.hsql` - HSQLDB
* `DatabaseUtils.props.msaccess` - MS Access (see the [Windows Databases](windows_databases.md) article for more information)
* `DatabaseUtils.props.mssqlserver` - MS SQL Server 2000
* `DatabaseUtils.props.mssqlserver2005` - MS SQL Server 2005
* `DatabaseUtils.props.mysql` - MySQL
* `DatabaseUtils.props.odbc` - ODBC access via Sun's ODBC/JDBC bridge, e.g., for MS Sql Server (see the [Windows Databases](windows_databases.md) article for more information)
* `DatabaseUtils.props.oracle` - Oracle 10g
* `DatabaseUtils.props.postgresql` - PostgreSQL 7.4
* `DatabaseUtils.props.sqlite3` - sqlite 3.x

The easiest way is just to place the extracted properties file into your HOME directory. For more information on how property files are processed, check out [this](properties_file.md) article.

**Note:** Weka *only* looks for the `DatabaseUtils.props` file. If you take one of the example files listed above, you need to rename it first.

# Setup
Under normal circumstances you only have to edit the following two properties:

* `jdbcDriver`
* `jdbcURL`

## Driver
`jdbcDriver` is the classname of the JDBC driver, necessary to connect to your database, e.g.:

* HSQLDB - `org.hsqldb.jdbcDriver`
* MS SQL Server 2000 (Desktop Edition) - `com.microsoft.jdbc.sqlserver.SQLServerDriver`
* MS SQL Server 2005 - `com.microsoft.sqlserver.jdbc.SQLServerDriver`
* MySQL - `org.gjt.mm.mysql.Driver` (or `com.mysql.jdbc.Driver`)
* ODBC - part of Sun's JDKs/JREs, no external driver necessary - `sun.jdbc.odbc.JdbcOdbcDriver`
* Oracle - `oracle.jdbc.driver.OracleDriver`
* PostgreSQL - `org.postgresql.Driver`
* sqlite 3.x - `org.sqlite.JDBC`

## URL
`jdbcURL` specifies the JDBC URL pointing to your database (can be still changed in the Experimenter/Explorer), e.g. for the database `MyDatabase` on the server `server.my.domain`:

* HSQLDB - `jdbc:hsqldb:hsql://server.my.domain/MyDatabase`
* MS SQL Server 2000 (Desktop Edition) - `jdbc:microsoft:sqlserver://server.my.comain:1433`

    * Note: if you add `;databasename=*db-name*` you can connect to a different database than the default one, e.g., *MyDatabase*

* MS SQL Server 2005 - `jdbc:sqlserver://server.my.domain:1433`
* MySQL - `jdbc:mysql://server.my.domain:3306/MyDatabase`
* ODBC - `jdbc:odbc:DSN_name (replace DSN_name with the DSN that you want to use)`
* Oracle (thin driver) - `jdbc:oracle:thin:@server.my.domain:1526:orcl`

    * Note: `@machineName:port:SID`
    * for the *Express Edition* you can use: `jdbc:oracle:thin:@server.my.domain:1521:XE`

* PostgreSQL - `jdbc:postgresql://server.my.domain:5432/MyDatabase`

    * You can also specify user and password directly in the URL: `jdbc:postgresql://server.my.domain:5432/MyDatabase?user=<...>&password=<...>`
    * where you have to replace the `<...>` with the correct values

* sqlite 3.x - `jdbc:sqlite:/path/to/database.db (you can access only local files)`

# Missing Datatypes
Sometimes (e.g. with MySQL) it can happen that a column type cannot be interpreted. In that case it is necessary to map the name of the column type to the Java type it should be interpreted as. E.g. the MySQL type `TEXT` is returned as `BLOB` from the JDBC driver and has to be mapped to `String` (`0` represents `String` - the mappings can be found in the comments of the properties file):

```
 BLOB=0
```

The article [weka/experiment/DatabaseUtils.props](weka_experiment_database_utils.props.md) contains more details on this topic.

# Stored Procedures
Let's say you're tired of typing the same query over and over again. A good way to shorten that, is to create a stored procedure.

## PostgreSQL 7.4.x
The following example creates a procedure called **emplyoee_name** that returns the names of all the employees in table **employee**. Even though it doesn't make much sense to create a stored procedure for this query, nonetheless, it shows how to create and call stored procedures in PostgreSQL.

* Create

    ```
    CREATE OR REPLACE FUNCTION public.employee_name()
      RETURNS SETOF text AS 'select name from employee'
      LANGUAGE 'sql' VOLATILE;
    ```

* SQL statement to call procedure

    ```
    SELECT * FROM employee_name()
    ```

* Retrieve data via [InstanceQuery](http://weka.sourceforge.net/doc.dev/weka/experiment/InstanceQuery.html)

    ```
    java weka.experiment.InstanceQuery -Q "SELECT * FROM employee_name()" -U <user> -P <password>
    ```

# Troubleshooting
* In case you're experiencing problems connecting to your database, check out the [mailing list](mailing_list.md). It is possible that somebody else encountered the same problem as you and you'll find a post containing the solution to your problem.
* Specific  [MS SQL Server 2000](ms_sql_server_2000_desktop_engine.md#TroubleShooting) Troubleshooting
* MS SQL Server 2005: TCP/IP is not enabled for SQL Server, or the server or port number specified is incorrect.Verify that SQL Server is listening with TCP/IP on the specified server and port. This might be reported with an exception similar to: "The login has failed. The TCP/IP connection to the host has failed." This indicates one of the following:

    * SQL Server is installed but TCP/IP has not been installed as a network protocol for SQL Server by using the SQL Server Network Utility for SQL Server 2000, or the SQL Server Configuration Manager for SQL Server 2005
    * TCP/IP is installed as a SQL Server protocol, but it is not listening on the port specified in the JDBC connection URL. The default port is 1433.
    * The port that is used by the server has not been opened in the firewall

* The **Added driver: ...** output on the commandline does not mean that the actual class was found, but only that Weka will *attempt* to load the class later on in order to establish a database connection.
* The error message **No suitable driver** can be caused by the following:

    * The JDBC driver you are attempting to load is not in the [CLASSPATH](classpath.md) (Note: using **-jar** in the java commandline **overwrites** the CLASSPATH environment variable!). Open the SimpleCLI, run the command `java weka.core.SystemInfo` and check whether the property `java.class.path` lists your database jar. If not correct your [CLASSPATH](classpath.md) or the Java call you start Weka with.
    * The JDBC driver class is misspelled in the `jdbcDriver` property or you have multiple entries of `jdbcDriver` ([properties file](properties_file.md)s need *unique* keys!)
    * The `jdbcURL` property has a spelling error and tries to use a non-existing protocol or you listed it multiple times, which doesn't work either (remember, [properties file](properties_file.md)s need *unique* keys!)

# See also
* [weka/experiment/DatabaseUtils.props](weka_experiment_database_utils.props.md)
* [properties file](properties_file.md)
* [CLASSPATH](classpath.md)

# Links
* HSQLDB

    * [homepage](http://hsqldb.sourceforge.net/)

* IBM Cloudscape

    * [homepage](http://www.cloudscape.com/)

* Microsoft SQL Server

    * [SQL Server 2000 (Desktop Engine)](http://www.microsoft.com/downloads/details.aspx?FamilyID=8E2DFC8D-C20E-4446-99A9-B7F0213F8BC5)
    * [SQL Server 2000 JDBC Driver SP 3](http://www.microsoft.com/downloads/details.aspx?familyid=07287B11-0502-461A-B138-2AA54BFDC03A)
    * [SQL Server 2005 JDBC Driver](http://www.microsoft.com/downloads/details.aspx?familyid=e22bc83b-32ff-4474-a44a-22b6ae2c4e17&displaylang=en)

* MySQL

    * [homepage](http://www.mysql.com/)
    * [JDBC driver](http://dev.mysql.com/downloads/connector/j/)

* Oracle 

    * [homepage](http://www.oracle.com/)
    * [JDBC driver](http://www.oracle.com/technology/software/tech/java/sqlj_jdbc/)
    * [JDBC FAQ](http://www.orafaq.com/faqjdbc.htm)

* PostgreSQL
    
    * [homepage](http://www.postgresql.org/)
    * [JDBC driver](http://jdbc.postgresql.org/)

* sqlite

    * [homepage](http://www.sqlite.org/)
    * [JDBC driver](http://www.zentus.com/sqlitejdbc/)

* [Weka Mailing list](mailing_list.md)

