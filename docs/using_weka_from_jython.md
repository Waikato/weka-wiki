

Jython is an implementation of the high-level, dynamic, object-oriented language Python written in 100% Pure Java, and seamlessly integrated with the Java platform. It thus allows you to run Python on any Java platform.

*-- taken from the [Jython homepage](http://www.jython.org/)*

This article explains how use Weka classes from within Jython and how to write a classifier in Jython that can be used within the Weka framework.

# Accessing Weka classes from Jython
## Requirements

In order for Jython to find the Weka classes, you must export them in your [CLASSPATH](classpath.md). Here is an example for adding the `weka.jar` located in the directory `/some/where` to the CLASSPATH in a bash under Linux:

```bash
export CLASSPATH=$CLASSPATH:/some/where/weka.jar
```

**Note:** Windows users must just the backslash ("\") in the command prompt instead of the slash ("/") in paths.

## Implementation

As soon as one imports classes in a Jython module one can use that class just like in Java. E.g., if one wants to use the J48 classifier, one only needs to import it as follows:

```python
 import weka.classifiers.trees.J48 as J48
```

Here's a Jython module ([UsingJ48.py](files/UsingJ48.py)):

```python
 import sys
 
 import java.io.FileReader as FileReader
 import weka.core.Instances as Instances
 import weka.classifiers.trees.J48 as J48
 
 # load data file
 file = FileReader("/some/where/file.arff")
 data = Instances(file)
 data.setClassIndex(data.numAttributes() - 1)
 
 # create the model
 j48 = J48()
 j48.buildClassifier(data)
 
 # print out the built model
 print j48
```

A slightly more elaborate example can be found in [UsingJ48Ext.py](files/UsingJ48Ext.py), which uses more methods of the `weka.classifiers.Evaluation` class.

**NB:** The example `UsingJ48Ext.py` needs Weka 3.6.x to run, due to some changes in the API.

# Implementing a Jython classifier

## Requirements

* Weka >3.5.6 (or [snapshot](snapshots.md) from 02/08/2007 or later)
* Jython 2.2rc2 (later versions should work as well)

## Implementation

This section covers the implementation of `weka.classifiers.rules.ZeroR` in Python, [JeroR.py](files/JeroR.py):

* Subclass an abstract superclass of Weka classifiers (in this case `weka.classifiers.Classifier`):

> `class JeroR (**Classifier**, JythonSerializableObject):`

> *Note:* the `JythonSerializableObject` interface is necessary for Serialization purposes (Weka creates copies of classifiers via [serialization](serialization.md))

* You have to implement the following methods:

	* `def listOptions(self):`
>> Returns an `java.util.Enumeration` of `weka.core.Option` objects of all available options. Calling the superclass method is done with `*<superclass>*.listOptions()`, e.g., `Classifier.listOptions()`.
	* `def setOptions(self, options):`
>> Sets the commandline options, with the parameter `options` being an array of strings.
	* `def getOptions(self):`
>> Returns an array of strings, containing all the currently set options (to be used with `setOptions(self,options)`).
	* `def getCapabilities(self):`
>> Returns a `weka.core.Capabilities` object with information about what attributes and classes can be processed by this algorithm.
	* `def buildClassifier(self, instances):`
>> This method builds the actual model based on the data provided. The first statements in this method should be the ones checking the capabilities of the algorithm against the data and removing all instances with a missing class value:
```python
# check the capabilities
self.getCapabilities().testWithFail(instances)
# remove instances with missing class
instances = Instances(instances)
instances.deleteWithMissingClass()
```


* at least **one** of the following two:
		
	* `def classifyInstance(self, instance):`
>> Returns either the index of the predicted class label (for nominal classes) or the regression result (for numeric classes)
	* `def distributionForInstance(self, instance):`
>> This method returns an array of doubles containing the probabilities for all class labels. In case of a numeric class attribute, the length of this array is 1. In Jython, you can use the `[jarray](http://www.jython.org/docs/jarray.html)` module to generate a double array. With the following line you can create the correct array to be returned by this method (you still need to fill it with values):

>> `result = jarray.zeros(instance.numClasses(), 'd')`

>> Of course, the elements of this array must sum up to 1.
	* `def toString(self):`
>> Returns a string describing the not-yet-built or built model.

* The following code snippet simulates the "main" method; it creates an instance of the classifier and passes it on to the `Classifier.runClassifier` method:

```python
if __name__ = "__main__":

    Classifier.runClassifier(JeroR(), sys.argv[1:])
```
> This doesn't work right out-of-the-box, since Jython cannot access protected static methods in superclasses. One has to set the following value in the [Jython registry](https://www.jython.org/archive/21/docs/registry.html) to make it work (taken from [this](http://www.jython.org/cgi-bin/faqw.py?req=all#3.5) FAQ):

```ini
python.security.respectJavaAccessibility=false
```

## Documentation
Documentation in Python is done with the so-called *doc strings* within the class or method the documentation is for. Using [HappyDoc](http://happydoc.sourceforge.net/), one can use *structured text* to output nice HTML, similar to [Javadoc](http://java.sun.com/j2se/javadoc/).

* Class doc string:

```python
 class JeroR (Classifier, JythonSerializableObject):

     """
     JeroR is a Jython implementation of the Weka classifier ZeroR
     
     'author' -- FracPete (fracpete at waikato dot ac dot nz)
     
     'version' -- $Revision$
     """
```
> **Note:** the `$Revision$` tag is filled in by a source control system like CVS or [Subversion](subversion.md).

* Method doc string:

```python
 def classifyInstance(self, instance):

     """
     returns the prediction for the given instance
     
     Parameter(s):

     
         'instance' -- the instance to predict the class value for
     
     Return:

     
         the prediction for the given instance
     """
```

## Execution
**Note:** The commands listed here for a Linux/Unix bash, for Windows remove all the backslashes ("\") at the end of the lines and assemble the command in a single line. Under Windows, the path separator ":" used in the [CLASSPATH](classpath.md) needs to be replaced with ";" as well.

## Jython
The Jython classifier, e.g., `FunkyClassifier.py`, can be run like this from commandline, with only the `weka.jar` and the `jython.jar` in the [CLASSPATH](classpath.md):

```bash
 java -classpath weka.jar:jython.jar \
   org.python.util.jython \
   /some/place/FunkyClassifier.py \
   -t /some/where/file.arff
```

## Weka
In order to execute the Jython classifier `FunkyClassifier.py` with Weka, one basically only needs to have the `weka.jar` and the `jython.jar` in the [CLASSPATH](classpath.md) and call the `weka.classifiers.JythonClassifier` classifier with the Jython classifier, i.e., `FunkyClassifier.py`, as parameter ("`-J`"):

```bash
 java -classpath weka.jar:jython.jar \
   weka.classifiers.JythonClassifier \
   -J /some/place/FunkyClassifier.py \
   -t /some/where/file.arff
```

# Downloads
* [UsingJ48.py](files/UsingJ48.py)
* [UsingJ48Ext.py](files/UsingJ48Ext.py)
* [JeroR.py](files/JeroR.py) - `weka.classifiers.rules.ZeroR` as Jython script implemented

# See also
* [Use Weka in your Java code](use_weka_in_your_java_code.md) - for general information on how to use the Weka API
* [Using Weka via Jepp](using_weka_via_jepp.md) - using the `javax.script` approach to interface Java and Python

# Links
* Jython
	* [Homepage](http://www.jython.org/)
	* [Java arrays](https://www.jython.org/archive/21/docs/jarray.html)
	* [Registry](https://www.jython.org/archive/21/docs/registry.html)
	* [Embedding Jython](https://www.jython.org/archive/21/docs/embedding.html)
* Python
	* [Homepage](http://www.python.org/)
	* [HappyDoc](http://happydoc.sourceforge.net/) - generating documentation from Jython/Python modules
* Java
	* [Homepage](http://java.sun.com)
	* [Javadoc](http://java.sun.com/j2se/javadoc/)
* Eclipse
	* [Homepage](http://www.eclipse.org/)
	* [PyDev plugin](http://pydev.sourceforge.net/)
