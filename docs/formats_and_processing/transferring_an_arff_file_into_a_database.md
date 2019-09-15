
This example transfers a dataset stored in an [ARFF](arff.md) file into the MySQL [database](../databases.md) *weka_test* on the MySQL server running on the same machine. In order to make this work, the MySQL JDBC driver must be in the [CLASSPATH](../classpath.md) and the [DatabaseUtils.props](../weka_experiment_database_utils.props.md) file must be configured accordingly.

Usage:

```bash
 Arff2Database <input.arff>
```
Source code:

```java
import weka.core.*;
  import weka.core.converters.*;
  import java.io.*;
  
  /**
   * A simple API example of transferring an ARFF file into a MySQL table.
   * It loads the data into the database "weka_test" on the MySQL server
   * running on the same machine. Instead of using the relation name of the
   * database as the table name, "mytable" is used instead. The
   * DatabaseUtils.props file must be configured accordingly.
   *
   * Usage: Arff2Database input.arff
   *
   * @author FracPete (fracpete at waikato dot ac dot nz)
   */
  public class Arff2Database {
    
    /**
     * loads a dataset into a MySQL database
     *
     * @param args    the commandline arguments
     */
    public static void main(String[] args) throws Exception {
      Instances data = new Instances(new BufferedReader(new FileReader(args[0])));
      data.setClassIndex(data.numAttributes() - 1);
      
      DatabaseSaver save = new DatabaseSaver();
      save.setUrl("jdbc:mysql://localhost:3306/weka_test");
      save.setUser("fracpete");
      save.setPassword("fracpete");
      save.setInstances(data);
      save.setRelationForTableName(false);
      save.setTableName("mytable");
      save.connectToDatabase();
      save.writeBatch();
    }
  }
```

# See also
* [Databases](../databases.md) - explains how to use databases within Weka
* [weka/experiment/DatabaseUtils.props](../weka_experiment_database_utils.props.md) - the properties file explained in detail

# Downloads
* [Arff2Database.java](../files/Arff2Database.java)
* The [Weka Examples](../weka_examples.md) collection contains several example classes:

	* `SaveDataToDbBatch.java` ([book](https://svn.cms.waikato.ac.nz/svn/weka/branches/book2ndEd-branch/wekaexamples/src/main/java/wekaexamples/core/converters/SaveDataToDbBatch.java), [stable-3.6](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-6/wekaexamples/src/main/java/wekaexamples/core/converters/SaveDataToDbBatch.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/core/converters/SaveDataToDbBatch.java))
	* `SaveDataToDbIncremental.java` ([book](https://svn.cms.waikato.ac.nz/svn/weka/branches/book2ndEd-branch/wekaexamples/src/main/java/wekaexamples/core/converters/SaveDataToDbIncremental.java), [stable-3.6](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-6/wekaexamples/src/main/java/wekaexamples/core/converters/SaveDataToDbIncremental.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/core/converters/SaveDataToDbIncremental.java))

# Links
* [MySQL homepage](http://www.mysql.com/)
