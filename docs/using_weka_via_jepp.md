

Jepp embeds [CPython](http://www.python.org/) in Java. It is safe to use in a heavily threaded environment, it is quite fast and its stability is a main feature and goal. 

*--taken from the [Jepp homepage](http://jepp.sourceforge.net/)*

# Prerequisites
* Java 6 (jepp makes use of the `javax.script` package)
* Jepp 2.2 or higher
* [suggested fix](http://sourceforge.net/forum/message.php?msg_id=4706583) for the *missing sys.argv* problem

# Limitations
Jepp doesn't seem to be able to import third-party libraries like `scipy`, `numpy` or `wx` (pure Python modules can be imported, though).

# Accessing Weka classes within Jepp
Java classes are imported in one's Python script as follows:

```python
 from <package> import <class>
```
E.g., importing J48 looks like this:

```python
 from weka.classifiers.trees import J48
```
In the following a little example script for loading a dataset, cross-validating J48 with it and outputting the results of the cross-validation in the console:

```python
 # import classes
 from weka.core import Instances
 from weka.classifiers import Evaluation
 from weka.classifiers.trees import J48
 
 from java.io import BufferedReader
 from java.io import FileReader
 from java.util import Random
 
 # load data
 reader = BufferedReader(FileReader('/some/where/file.arff'))
 data   = Instances(reader)
 data.setClassIndex(data.numAttributes() - 1)
 reader.close()
 
 # train classifier
 j48  = J48()
 eval = Evaluation(data)
 rand = Random(1)
 eval.crossValidateModel(j48, data, 10, rand)
 
 # output summary
 print eval.toSummaryString()
```
The script can be started like this (you will have to adjust the paths for the jars and the Python script):

```bash
 java -classpath jep.jar:weka.jar some_script.py
```

# See also
* [Use Weka in your Java code](use_weka_in_your_java_code.md) - for general information on how to use the Weka API
* [Using Weka from Jython](using_weka_from_jython.md)

# Links
* [Jepp homepage](http://jepp.sourceforge.net/)
* [Python homepage](http://www.python.org/)
* [Linux.com](http://www.linux.com/feature/57950) - more examples
