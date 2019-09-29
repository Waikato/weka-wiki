
Remote experiments enable you to distribute the computing load across multiple computers. In the following we will discuss the setup and operation for [HSQLDB](http://hsqldb.sourceforge.net/) and [MySQL](http://www.mysql.com/).

# Preparation
To run a remote experiment you will need:

* A database server.
* A number of computers to run remote engines on.
* To edit the remote engine policy file included in the Weka distribution to allow class and dataset loading from your home directory.
* An invocation of the Experimenter on a machine somewhere (any will do).

For the following examples, we assume a user called *johndoe* with this setup:

* Access to a set of computers running a flavour of Unix (pathnames need to be changed for Windows).
* The home directory is located at `/home/johndoe`.
* Weka is found in `/home/johndoe/weka`.
* Additional jar archives, i.e., JDBC drivers, are stored in `/home/johndoe/jars`.
* The directory for the datasets is `/home/johndoe/datasets`.

**Note:** The example policy file `remote.policy.example` is using this setup (available in `weka/experiment`). 

# Database Server Setup
* HSQLDB
** Download the JDBC driver for HSQLDB, extract the `hsqldb.jar` and place it in the directory `/home/johndoe/jars`.
** To set up the database server, choose or create a directory to run the database server from, and start the server with:

```bash
 java -classpath /home/johndoe/jars/hsqldb.jar \
   org.hsqldb.Server -database.0 experiment -dbname.0 experiment
```
>  **Note:** This will start up a database with the alias *experiment* (`-dbname.0 <alias>`) and create a properties and a log file at the current location prefixed with *experiment* (`-database.0 <file>`).

* MySQL
> We won't go into details of setting up a MySQL server, but this is rather straightforward and includes the following steps:

** Download a suitable version of MySQL for your server machine.
** Start the MySQL server.
** Create a database - for our example we will use experiment as database name.
** Download the appropriate JDBC driver, extract the JDBC jar and place it as `mysql.jar` in `/home/johndoe/jars`.

# Remote Engine Setup
* First, set up a directory for scripts and policy files:

```bash
 /home/johndoe/remote_engine
```
* Unzip the `remoteExperimentServer.jar` (from the Weka distribution; or build it from the sources with `ant remotejar`) into a temporary directory.
* Next, copy the remoteEngine.jar to the `/home/johndoe/remote_engine` directory.
* Create a script, called `/home/johndoe/remote_engine/startRemoteEngine`, with the following content (don't forget to make it executable with `chmod a+x startRemoteEngine`):

> * HSQLDB
```bash
 java -Xmx256m \
   -classpath /home/johndoe/jars/hsqldb.jar:remoteEngine.jar \
   -Djava.security.policy=remote.policy \
   weka.experiment.RemoteEngine &
```
> * MySQL
```bash
 java -Xmx256m \
   -classpath /home/johndoe/jars/mysql.jar:remoteEngine.jar \
   -Djava.security.policy=remote.policy \
   weka.experiment.RemoteEngine &
```
> * From Weka 3.7.2 you will need to include the core weka.jar file in the classpath for the RemoteEngine. Assuming that the weka.jar file has been copied to `/home/johndoe/remote_engine`:

```bash
java -Xmx256m \
  -classpath /home/johndoe/jars/hsqldb.jar:remoteEngine.jar:weka.jar \
  -Djava.security.policy=remote.policy \
  weka.experiment.RemoteEngine &
```

* Now we will start the remote engines (note that the same version of Java must be used for the Experimenter and remote engines) :

	* Copy the `remote.policy.example` file to `/home/johndoe/remote_engine` as `remote.policy`.
	* For each machine you want to run an engine on:
	
		* `ssh` to the machine.
		* `cd` to `/home/johndoe/remote_engine`.
		* Run `/home/johndoe/startRemoteEngine` (to enable the remote engines to use more memory, modify the `-Xmx` option in the `startRemoteEngine` script) .

# Configuring the Experimenter
Now we will run the Experimenter:

* HSQLDB
	* Copy the *DatabaseUtils.props.hsql* file to the `/home/johndoe/remote_engine` directory and rename it to *DatabaseUtils.props* - a copy comes with your Weka distribution in `weka/experiment`.
	* Edit this file and change the "`jdbcURL=jdbc:hsqldb:hsql://server_name/database_name`" entry to include the name of the machine that is running your database server (e.g., `jdbcURL=jdbc:hsqldb:hsql://dodo.company.com/experiment`).
	* Now start the experimenter (inside this directory):

```bash
 java \
   -cp /home/johndoe/jars/hsqldb.jar:remoteEngine.jar:/home/johndoe/weka/weka.jar \
   -Djava.rmi.server.codebase=file:/home/johndoe/weka/weka.jar \
   weka.gui.experiment.Experimenter
```
* MySQL
	* Copy the *DatabaseUtils.props.mysql* file to the `/home/johndoe/remote_engine` directory and rename it to *DatabaseUtils.props* - a copy comes with your Weka distribution in `weka/experiment`.
	* Edit this file and change the "`jdbcURL=jdbc:mysql://server_name:3306/database_name`" entry to include the name of the machine that is running your database server and the name of the database the result will be stored in (e.g., `jdbcURL=jdbc:mysql://dodo.company.com:3306/experiment`).
	* Now start the experimenter (inside this directory):

```bash
 java \
   -cp /home/johndoe/jars/mysql.jar:remoteEngine.jar:/home/johndoe/weka/weka.jar \
   -Djava.rmi.server.codebase=file:/home/johndoe/weka/weka.jar \
   weka.gui.experiment.Experimenter
```
>  **Note:** the database name experiment can be still modified in the Experimenter, this is just the default setup. 

Now we will configure the experiment:

* First of all select the *Advanced* mode in the Setup tab
* Now choose the *DatabaseResultListener* in the *Destination* panel. Configure this result producer:

	* HSQLDB
>> Supply the value **sa** for the username and leave the password empty.
	* MySQL
>> Provide username and password that you need for connecting to the database.

* From the Result generator panel choose either the *CrossValidationResultProducer* or the *RandomSplitResultProducer* (these are the most commonly used ones) and then configure the remaining experiment details (e.g., datasets and classifiers).
* Now enable the *Distribute Experiment* panel by checking the tick box.
* Click on the *Hosts* button and enter the names of the machines that you started remote engines on (`<Enter>` adds the host to the list).
* You can choose to distribute by run or dataset (try to get a balance).
* Save your experiment configuration.
* Now start your experiment as you would do normally.
* Check your results in the *Analyse* tab by clicking either the *Database* or *Experiment* buttons.

# Multi-core support
If you want to utilize all the cores on a multi-core machine, then you can do so with Weka version 3.6.x and developer versions later than 3.5.7. All you have to do, is define the port alongside the hostname in the Experimenter (format: 
`hostname:port`) and then start the `RemoteEngine` with the `-p` option, specifying the port to listen on.
See also [this](https://list.waikato.ac.nz/pipermail/wekalist/2009-March/016310.html) post on the [Wekalist](../mailing_list.md).

# Troubleshooting
* If you get an error at the start of an experiment that looks a bit like this:

```
{{01:13:19: RemoteExperiment (//blabla.company.com/RemoteEngine) (sub)experiment (datataset vineyard.arff) failed : java.sql.SQLException: Table already exists: EXPERIMENT_INDEX in statement [CREATE TABLE Experiment_index ( Experiment_type LONGVARCHAR, Experiment_setup LONGVARCHAR, Result_table INT )]

01:13:19: dataset :vineyard.arff RemoteExperiment

       (//blabla.company.com/RemoteEngine) (sub)experiment (datataset vineyard.arff) failed : java.sql.SQLException: Table already exists: EXPERIMENT_INDEX in statement [CREATE TABLE Experiment_index ( Experiment_type LONGVARCHAR, Experiment_setup LONGVARCHAR, Result_table INT )]. Scheduling for execution on another host.}}
```
> then do not panic - this happens because multiple remote machines are trying to create the same table and are temporarily locked out - this will resolve itself so just leave your experiment running - in fact, it is a sign that the experiment is working!


* If you serialized an experiment and then modify your `DatabaseUtils.props` file due to an error (e.g., a missing type-mapping), the Experimenter will use the `DatabaseUtils.props` you had at the time you serialized the experiment. Keep in mind that the serialization process also serializes the DatabaseUtils class and therefore stored your props-file! This is another reason for storing your experiments as XML and not in the properietary binary format the Java serialization produces.
* Using a corrupt or incomplete `DatabaseUtils.props` file can cause peculiar interface errors, for example disabling the use of the *User* button alongside the database URL. If in doubt copy a clean `DatabaseUtils.props` from [Subversion](../subversion.md).
* If you get `NullPointerException at java.util.Hashtable.get()` in the Remote Engine do not be alarmed. This will have no effect on the results of your experiment.

# Links 
* [Databases](../databases.md)
* [weka/experiment/DatabaseUtils.props](../weka_experiment_database_utils.props.md)
* [Subversion](../subversion.md)
* [HSQLDB](http://hsqldb.sourceforge.net/)
* [MySQL](http://www.mysql.com/)
