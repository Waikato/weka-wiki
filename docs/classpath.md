The `CLASSPATH` environment variable tells Java where to look for classes. Since Java does the search in a ''first-come-first-serve'' kind of manner, you'll have to take care where and what to put in your CLASSPATH. I, personally, never use the environment variable, since I'm working often on a project in different versions in parallel. The CLASSPATH would just mess up things, if you're not careful (or just forget to remove an entry). [[ANT]] offers a nice way for building (and separating source code and class files) Java projects.
But still, if you're only working on totally separate projects, it might be easiest for you to use the environment variable.

# Setting the CLASSPATH
In the following we add the `mysql-connector-java-5.1.6-bin.jar` to our `CLASSPATH` variable (this works for any other jar archive) to make it possible to access MySQL [Databases](databases.md) via JDBC.

## Windows
We assume that the `mysql-connector-java-5.1.6-bin.jar` archive is located in the following directory:

```
C:\Program Files\Weka-3-8
```

In the *Control Panel* click on *System* (or right click on *This PC* and select *Properties*) and then go to the *Advanced* tab. There you will find a button called *Environment Variables*, click it.

Depending on, whether you're the only person using this computer or it is a lab computer shared by many, you can either create a new system-wide (you are the only user) environment variable or a user dependent one (recommended for multi-user machines). Enter the following name for the variable

```
CLASSPATH
```

and add this value

```
C:\Program Files\Weka-3-8\mysql-connector-java-5.1.6-bin.jar
```

If you want to add additional jars, you'll have to separate them with the path separator, the semicolon **;** (no spaces!).

## Unix/Linux
I assume, that the mysql jar is located in the following directory:

```
/home/johndoe/jars/
```

Open a shell and execute the following command, depending on the shell you're using:

* bash

   ```
   export CLASSPATH=$CLASSPATH:/home/johndoe/jars/mysql-connector-java-5.1.6-bin.jar
   ```

* c shell

   ```
   setenv CLASSPATH $CLASSPATH:/home/johndoe/jars/mysql-connector-java-5.1.6-bin.jar
   ```

Unix/Linux uses the colon **:** as path separator, in contrast to Windows, which uses the semicolon **;**.

**Note:** the prefixing with `$CLASSPATH` adds the mysql jar at the end of the currently existing `CLASSPATH`.

## Cygwin
The process is like with Unix/Linux systems, but since the host system is Win32 and therefore the Java installation also a Windows application, you'll have to use the semicolon **;** as separator for several jars.

