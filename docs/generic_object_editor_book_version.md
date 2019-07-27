

# Introduction
As of version 3.4.4 it is possible for WEKA to [dynamically discover classes at runtime](generic_object_editor_book_version.md#class-discovery) (rather than using only those specified in the `GenericObjectEditor.props` (GOE) file).

If dynamic class discovery is too slow, e.g., due to an enormous CLASSPATH, you can generate a new `GenericObjectEditor.props` file and then turn dynamic class discovery off. It is assumed that you already placed the `GenericPropertiesCreator.props` (GPC) file in your home directory (this file is located in directory `weka/gui` of either the `weka.jar` or `weka-src.jar` ZIP archive) and that the `weka.jar` jar archive with the WEKA classes is in your CLASSPATH (otherwise you have to add it to the `java` call using the `-classpath` option).

For generating the GOE file, execute the following steps:

* generate a new `GenericObjectEditor.props` file using the following command:

	* Linux/Unix
> `
java weka.gui.GenericPropertiesCreator \
   $HOME/GenericPropertiesCreator.props \
   $HOME/GenericObjectEditor.props
`

	* Windows (command must be in one line)
> `
java weka.gui.GenericPropertiesCreator 
   %USERPROFILE%\GenericPropertiesCreator.props
   %USERPROFILE%\GenericObjectEditor.props
`

* edit the `GenericPropertiesCreator.props` file in your home directory and set `UseDynamic` to `false`.

For disabling dynamic class discovery, you need to set the boolean constant `USE_DYNAMIC` of the `weka.gui.GenericObjectEditor` class to `false`. See article [Compiling WEKA](compiling_weka.md) for more information on how to compile a modified version of WEKA.

A limitation of the GOE prior to 3.4.4 was, that additional classifiers, filters, etc., had to fit into the same package structure as the already existing ones, i.e., all had to be located below `weka`. WEKA can now display multiple class hierarchies in the GUI, which makes adding new functionality quite easy as we will see later in an example (it is not restricted to classifiers only, but also works with all the other entries in the GPC file).

# File Structure
The structure of the GOE so far was a key-value-pair, separated by an *equals*-sign. The *value* is a comma separated list of classes that are all derived from the superclass/superinterface *key*. The GPC is slightly different, instead of declaring all the classes/interfaces one need only to specify all the packages descendants are located in (only non-abstract ones are then listed). E.g., the `weka.classifiers.Classifier` entry in the GOE file looks like this:

```ini
 weka.classifiers.Classifier=\
  weka.classifiers.bayes.AODE,\
  weka.classifiers.bayes.BayesNet,\
  weka.classifiers.bayes.ComplementNaiveBayes,\
  weka.classifiers.bayes.NaiveBayes,\
  weka.classifiers.bayes.NaiveBayesMultinomial,\
  weka.classifiers.bayes.NaiveBayesSimple,\
  weka.classifiers.bayes.NaiveBayesUpdateable,\
  weka.classifiers.functions.LeastMedSq,\
  weka.classifiers.functions.LinearRegression,\
  weka.classifiers.functions.Logistic,\
  ...
```
The entry producing the same output for the classifiers in the GPC looks like this (7 lines instead of over 70!):

```ini
 weka.classifiers.Classifier=\
  weka.classifiers.bayes,\
  weka.classifiers.functions,\
  weka.classifiers.lazy,\
  weka.classifiers.meta,\
  weka.classifiers.trees,\
  weka.classifiers.rules
```

# Class Discovery
Unlike the `Class.forName(String)` method that grabs the first class it can find in the [CLASSPATH](classpath.md), and therefore fixes the location of the package it found the class in, the dynamic discovery examines the complete [CLASSPATH](classpath.md) you're starting the [Java Virtual Machine](java_virtual_machine.md) (JVM) with. This means that you can have several parallel directories with the same WEKA package structure, e.g. the standard release of WEKA in one directory ( `/distribution/weka.jar`) and another one with your own classes (`/development/weka/...`), and display all of the classifiers in the GUI. In case of a name conflict, i.e. two directories contain the same class, the first one that can be found is used. In a nutshell, your `java` call of the GUIChooser could look like this:

```bash
 java -classpath "/development:/distribution/weka.jar" weka.gui.GUIChooser
```
*Note:* Windows users have to replace the ":" with ";" and the forward slashes with backslashes.


# Multiple Class Hierarchies
In case you're developing your own framework, but still want to use your classifiers within WEKA that wasn't possible so far. With the release **3.4.4** it is possible to have multiple class hierarchies being displayed in the GUI. If you've developed a modified version of J48, let's call it *MyJ48* and it's located in the package `dummy.classifiers` then you'll have to add this package to the classifiers list in the GPC file like this:

```ini
 weka.classifiers.Classifier=\
  weka.classifiers.bayes,\
  weka.classifiers.functions,\
  weka.classifiers.lazy,\
  weka.classifiers.meta,\
  weka.classifiers.trees,\
  weka.classifiers.rules,\
  dummy.classifiers
```
Your `java` call for the GUIChooser might look like this:

```bash
 java -classpath "weka.jar:dummy.jar" weka.gui.GUIChooser
```
*Note:* Windows users have to replace the ":" with ";" and the forward slashes with backslashes.

Starting up the GUI you'll now have another root node in the tree view of the classifiers, called *root*, and below it the *weka* and the *dummy* package hierarchy as you can see here:

# Links
* [GenericObjectEditor (developer version)](generic_object_editor_developer_version.md)
* [CLASSPATH](classpath.md)
* [Properties file](properties_file.md)
* [GenericPropertiesCreator.props](weka_gui_generic_properties_creator.props.md)
