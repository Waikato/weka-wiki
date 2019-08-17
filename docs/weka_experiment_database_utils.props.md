# File
`weka/experiment/DatabaseUtils.props`

# Description
Defines the [Databases](databases.md) setup, i.e., JDBC driver information, JDBC URL, database type conversion, etc.

# Version

* \>= 3.1.3

# Fields

* General

	* `jdbcDriver`
	
		the comma-separated list of jdbc drivers to try loading
		
	* `jdbcURL`
	
		the JDBC URL to the database
		
* Table creation

	* `CREATE_STRING`
	
		database specific datatype, e.g., `TEXT`
		
	* `CREATE_INT`
	
		database specific datatype, e.g., `INT`
		
	* `CREATE_DOUBLE`
	
		database specific datatype, e.g., `DOUBLE`
		
* Database flags

	* `checkUpperCaseNames`
	
		necessary if database turns column names into upper case ones, e.g., HSQLDB
		
	* `checkLowerCaseNames` (> 3.5.3)
	
		necessary if database turns column names into lower case ones, e.g., PostgreSQL
		
	* `checkForTable` (> 3.5.3)
	
		Checks whether the tables in the query are available in the meta-data of the JDBC Connection. Some tables, like `pg_tables`, exist but are not available through the meta-data
		
	* `setAutoCommit`
	
		setting for `java.sql.Connection.setAutoCommit(boolean)`
		
	* `createIndex`
	
		whether to create a primary key `Key_IDX` in the results table of an experiment
		
* Special flags for DatabaseLoader/Saver (package `weka.core.converters`)

	* `nominalToStringLimit` (>= 3.4.1)
	
		beyond this limit, nominal columns are loaded as STRING attributes and no longer as NOMINAL ones
		
	* `idColumn` (>= 3.4.1)
	
		unique key in table that allows ordering for incremental loading
		
	* `Keywords` (> 3.5.8, > 3.6.0)
	
		lists all the reserved keywords of the current database type
		
		default: `AND,ASC,BY,DESC,FROM,GROUP,INSERT,ORDER,SELECT,UPDATE,WHERE`
		
	* `KeywordsMaskChar` (> 3.5.8, > 3.6.0)
	
		the character to append to attribute names/table names that would be interpreted as keywords by the database, in order to avoid exceptions when executing SQL commands
		defaut: `_`

# Database type mapping
In order to import the data from database correctly into Weka, one has to specify what JDBC datatype corresponds to what Java SQL retrieval method. Here's an overview of how the Java types are mapped to Weka's attribute types:

| **Java type** | **Java method** | **Identifier** | **Weka attribute type** | **Version** |
--- | --- | --- | --- | --- |
String | getString() | 0 | nominal | 
boolean | getBoolean() | 1 | nominal | 
double | getDouble() | 2 | numeric | 
byte | getByte() | 3 | numeric | 
short | getByte() | 4 | numeric | 
int | getInteger() | 5 | numeric | 
long | getLong() | 6 | numeric | 
float | getFloat() | 7 | numeric | 
date | getDate() | 8 | date | 
text | getString() | 9 | string | >3.5.5 
time | getTime() | 10 | string | >3.5.8 

In the props file one lists now the type names that the database returns and what Java type it represents (via the identifier), e.g.:
```ini
 CHAR=0
 VARCHAR=0
```

`CHAR` and `VARCHAR` are both String types, hence they are interpreted as *String* (identifier **0**)

**Note:** in case database types have blanks, one needs to replace those blanks with an underscore, e.g., `DOUBLE PRECISION` must be listed like this:
```ini
 DOUBLE_PRECISION=2
```

# See also
* [Databases](databases.md)
* [Properties file](properties_file.md)
