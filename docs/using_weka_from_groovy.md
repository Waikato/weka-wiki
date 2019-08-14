
Groovy...

* is an agile and dynamic language for the Java Virtual Machine
* builds upon the strengths of Java but has additional power features inspired by languages like Python, Ruby and Smalltalk
* makes modern programming features available to Java developers with almost-zero learning curve
* supports Domain-Specific Languages and other compact syntax so your code becomes easy to read and maintain
* makes writing shell and build scripts easy with its powerful processing primitives, OO abilities and an Ant DSL
* increases developer productivity by reducing scaffolding code when developing web, GUI, database or console applications
* simplifies testing by supporting unit testing and mocking out-of-the-box
* seamlessly integrates with all existing Java objects and libraries
* compiles straight to Java bytecode so you can use it anywhere you can use Java

*-- taken from the [Groovy homepage](https://groovy-lang.org/)*

This article explains how to use Weka classes within Groovy.

# Groovy CLASSPATH 
Additional jars can be added to Groovy in various ways:

* **`$GROOVY_HOME/lib`**
> Any jar that is placed in this directory will be available in Groovy
* **`$GROOVY_HOME/conf/groovy-starter.conf`**
> This file lists jar files and directories from which to include jar files. The syntax is fairly easy:

>> `load <path>`

>> For example, loading the `weka.jar` that is located in $HOME/myjars:

>> `load !{user.home}/myjars/weka.jar`

>> Or loading all jars in a directory, e.g., $HOME/myjars:

>> `load !{user.home}/myjars/*.jar`

* **Java `CLASSPATH`**

> Groovy automatically imports the Java [CLASSPATH](classpath.md), i.e., everything that is listed in the CLASSPATH environment variable.

* **`-classpath option`**

> When running a Groovy script, you can explicitly tell Groovy, what CLASSPATH to use:

>> `groovy -classpath <jars, etc=""> <script.groovy>`

# The Grape: 

Groovy dependency manager 

Grape is a JAR dependency manager embedded into Groovy. Grape lets you quickly add maven repository dependencies to your classpath, making scripting even easier. Weka can be added as a dependency in your Groovy script

You can also search for dependencies and versions numbers on [mvnrepository.com](http://mvnrepository.com/) and it will provide you the `Grab` annotation form of the `pom.xml` entry.

# Accessing Weka classes from Groovy 

## Requirements

* Groovy 1.5.7 or later
* Weka 3.5.4 or later

## Implementation

If your Groovy CLASSPATH has been setup correctly, you can use all those classes in Groovy straight away. E.g., after including the `weka.jar`, I can run the following little script ([UsingJ48.groovy](files/UsingJ48.groovy) to train J48 on a supplied dataset and output its model:

```groovy
import weka.classifiers.trees.J48
import weka.core.converters.ConverterUtils.DataSource
import weka.core.Instances

if (args.size() == 0) {
  println "Usage: UsingJ48.groovy <arff-file>"
  System.exit(0)
}

// load data and set class index
data = DataSource.read(args[0])
data.setClassIndex(data.numAttributes() - 1)

// create the model
j48 = new J48()
j48.buildClassifier(data)

// print out the built model
println j48
```
A slightly more elaborate example can be found in [UsingJ48Ext.groovy](files/UsingJ48Ext.groovy), which uses more methods of the `weka.classifiers.Evaluation` class.

**NB:** The example `UsingJ48Ext.groovy` needs Weka 3.6.x (or a snapshot of the developer version) to run, due to some changes in the API.

# Implementing a Groovy classifier 

## Requirements

* Groovy 1.5.7 or later
* developer version of Weka later than 3.5.8, [snapshots](snapshots.md) later than 02/02/2009

## Implementation
Implementing a Groovy classifier is pretty straight-forward, since the syntax is almost the same and Java classes are imported/used in Groovy just like in Java. The [GeroR.groovy](files/GeroR.groovy) file re-implements the `weka.classifiers.rules.ZeroR` classifier as Groovy script.

The class declaration for `GeroR.groovy` looks like this, for instance:

```groovy
class GeroR
  extends Classifier
  implements WeightedInstancesHandler {
    ...
}
```

For more information on implementing classifiers, see the [Writing your own Classifier](writing_classifier.md) article.

## Execution

### Groovy 

As long as you have the `weka.jar` in your Groovy environment, you can directly run these scripts using the `groovy` command.
Here is an example, executing the `FunkyClassifier.groovy` script in a Linux bash:

```bash
groovy -classpath weka.jar \
  /some/where/FunkyClassifier.groovy \
  -t /my/datasets/data.arff \
  <more options for the Groovy script>
```

### Weka
In order to execute classifiers written in Groovy, you have to use the `weka.classifiers.scripting.GroovyClassifier` classifier.
Here is an example, executing the `FunkyClassifier.groovy` script in a Linux bash:

```bash
java weka.jar:groovy-all-1.5.7.jar \
  weka.classifiers.scripting.GroovyClassifier \
  -t /my/datasets/data.arff \
  -G /some/where/FunkyClassifier.groovy \
  -- <more options for the Groovy script>
```

# Downloads 
* [UsingJ48.groovy](files/UsingJ48.groovy) - simple example for using J48 in a Groovy script
* [UsingJ48Ext.groovy](files/UsingJ48Ext.groovy) - a slightly more elaborate example script for using J48
* [CustomCV.groovy](files/CustomCV.groovy) thread
* [GeroR.groovy](files/GeroR.groovy) - `weka.classifiers.rules.ZeroR` implemented in Groovy
* [LibsvmWeights.groovy](files/LibsvmWeights.groovy) and outputs the best weights
* [multiple_eval.groovy](files/multiple_eval.groovy) - evaluates multiple classifiers on multiple train/test pairs and outputs statistics
* [AttributeStatistics.groovy](files/AttributeStatistics.groovy) - Outputs statistics for each attribute in a dataset

# Links 

* Groovy
	* [Homepage](https://groovy-lang.org/)
	* [Getting Started](https://groovy-lang.org/learn.html)
	* [Style Guide](https://groovy-lang.org/style-guide.html)
	* [Support](https://groovy-lang.org/support.html)