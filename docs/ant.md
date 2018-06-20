What is ANT? This is how the ANT [homepage](http://ant.apache.org/) defines its tool:

*Apache Ant is a Java-based build tool. In theory, it is kind of like Make, but without Make's wrinkles.*

# Basics

* the ANT build file is based on [XML](http://www.w3.org/XML/)
* the usual name for the build file is `build.xml`
* invocation - the usual build file needs not be specified explicitly, if it's in the current directory; if not target is specified, the default one is used

  ```
  ant [-f <build-file>] [<target>]
  ```

* displaying all the available targets of a build file

  ```
  ant [-f <build-file>] -projecthelp
  ```

# Weka and ANT

* a build file for Weka is available from [subversion](subversion.md) (it has been included in the `weka-src.jar` since version 3.4.8 and 3.5.3)
* it is located in the `weka` directory
* some targets of interest

  * `clean` - Removes the build, dist and reports directories; also any class files in the source tree
  * `compile` - Compile weka and deposit class files in `${path_modifier}/build/classes`
  * `docs` - Make javadocs into {${path_modifier}/doc}}
  * `exejar` - Create an executable jar file in `${path_modifier}/dist`


# Links

* [ANT homepage](http://ant.apache.org/)
* [XML](http://www.w3.org/XML/)

