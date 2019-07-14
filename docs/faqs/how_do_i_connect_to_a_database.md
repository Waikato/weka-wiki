With a bit of effort you can easily access databases via [JDBC](http://en.wikipedia.org/wiki/JDBC). You need the following:

* JDBC driver for the database you want to access in your CLASSPATH.
* A customized [DatabaseUtils.props](../weka_experiment_database_utils.props.md) file. The following example files are located in the `weka/experiment` directory of the `weka.jar` archive:
	* HSQLDB - `DatabaseUtils.props.hsql` (>= 3.4.1/3.5.0)
    * MS SQL Server 2000 - `DatabaseUtils.props.mssqlserver` (>= 3.4.9/3.5.4)
    * MS SQL Server 2005 Express Edition - `DatabaseUtils.props.mssqlserver2005` (> 3.4.10/3.5.5)
    * MySQL - `DatabaseUtils.props.mysql` (>= 3.4.9/3.5.4)
    * ODBC - `DatabaseUtils.props.odbc` (>= 3.4.9/3.5.4)
    * Oracle - `DatabaseUtils.props.oracle` (>= 3.4.9/3.5.4)
    * PostgreSQL - `DatabaseUtils.props.postgresql` (>= 3.4.9/3.5.4)
    * Sqlite 3.x - `DatabaseUtils.props.sqlite3` (> 3.4.12, > 3.5.7)

For more details see the following articles:

* [Databases](../databases.md)
* [DatabaseUtils.props](../weka_experiment_database_utils.props.md)
* [Windows databases](../windows/databases) (covers access via ODBC)

The following FAQs could be of interest as well:

* [Couldn't read from database: unknown data type](couldnt_read_from_database_unknown_data_type.md)
* [Trying to add JDBC driver: ... - Error, not in CLASSPATH?](trying_to_add_jdbc_driver_error_not_in_classpath.md)