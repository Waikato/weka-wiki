In case you have a flash idea for a new classifier and want to write one for Weka, this HOWTO will help you developing it. 

The Mindmap ([Build_classifier.pdf](files/Build_classifier.pdf), produced with [FreeMind](http://freemind.sourceforge.net)) helps you decide from which base classifier to start, what methods are to be implemented and general guidelines.

The base classifiers are all located in the following package:

```
 weka.classifiers
```

**Note:** This is also covered in chapter *Extending WEKA* of the WEKA manual.

# Packages
A few comments about the different classifier sub-packages:

* `bayes` - contains bayesian classifiers, e.g. NaiveBayes
* `evaluation` - classes related to evaluation, e.g., cost matrix
* `functions` - e.g., Support Vector Machines, regression algorithms, neural nets
* `lazy` - no *offline* learning, that is done during runtime, e.g., k-NN
* `meta` - Meta classifiers that use a *base* classifier as input, e.g., boosting or bagging
* `mi` - classifiers that handle multi-instance data
* `misc` - various classifiers that don't fit in any another category
* `rules` - rule-based classifiers, e.g. ZeroR
* `trees` - tree classifiers, like decision trees

# Coding
In the following you'll find notes about certain implementation parts listed in the Mindmap, which need a bit more explanation.

## Random number generators 
In order to get repeatable experiments, one is not allowed to use *unseeded* random number generators like `Math.random()`. Instead, one has to instantiate a `java.util.Random` object in the `buildClassifier(Instances)` method with a specific seed value. The seed value can be user supplied, of course, which all the `Randomizable...` abstract classifiers already implement.

## Capabilities
In old versions of Weka (up to version 3.5.2), all classifiers could handle basically every kind of data by default, unless they were throwing an Exception (in the `buildClassifier(Instances)` method). Since this behavior makes it cumbersome to introduce new attribute types, for instance (*all* classifiers have to be modified, which can't handle the new attribute type!), the general `Capabilities` were introduced.

## Base-classifier
Normal classifiers only state what kind of attributes and what kind of classes they can handle.

The `getCapabilities()` method of `weka.classifiers.trees.RandomTree`, for instance, looks like this:

```java
  public Capabilities getCapabilities() {
    Capabilities result = super.getCapabilities();   // returns the object from weka.classifiers.Classifier
    
    // attributes
    result.enable(Capability.NOMINAL_ATTRIBUTES);
    result.enable(Capability.NUMERIC_ATTRIBUTES);
    result.enable(Capability.DATE_ATTRIBUTES);
    result.enable(Capability.MISSING_VALUES);
    
    // class
    result.enable(Capability.NOMINAL_CLASS);
    result.enable(Capability.MISSING_CLASS_VALUES);
    
    return result;
  }
```

**Special cases:**

* **incremental classifiers** - By default, at least 1 instance has to be in the dataset, which does not apply for incremental classifiers. They have to lower the limit to `0`:

    ```
    result.setMinimumNumberInstances(0); 
    ```

* **multi-instance classifiers** - The structure for multi-instance classifiers is always fixed to *bagID,bag-data,class*. To restrict the data to multi-instance data, add the following:

    ```
    result.enable(Capability.ONLY_MULTIINSTANCE);
    ```

    Multi-instance classifiers also implement the following interface, which returns the Capabilities for the bag-data, which is just a *relational* attribute (the reason why `RELATIONAL_ATTRIBUTES` has to be enabled):

    ```
    weka.core.MultiInstanceCapabilitiesHandler
    ```

* **clusterer** - Since clusterer don't need a class attribute like classifiers, the following Capability has to be specified to enable datasets without a class attribute (which is already done in the superclass `weka.clusterers.Clusterer`):

    ```
    result.enable(Capability.NO_CLASS);
    ```

## Meta-classifier
Meta-classifiers, by default, just return the capabilities of their base classifiers - in case of descendants of the `weka.classifier.MultipleClassifiersCombiner`, an **AND** over all the Capabilities of the base classifiers is returned.

Due to this behavior, the Capabilities depend (normally) only on the currently configured base classifier(s). To *soften* filtering for certain behavior, meta-classifiers also define so-called *Dependencies* on a per-Capability basis. These dependencies tell the filter that even though a certain capability is not supported right now, it is possible that it will be supported with a different base classifier. By default, all Capabilities are initialized as Dependencies. 

`weka.classifiers.meta.LogitBoost`, e.g., is restricted to nominal classes. For that reason it disables the Dependencies for the class:

```java
    result.disableAllClasses();               // disable all class types
    result.disableAllClassDependencies();     // no dependencies!
    result.enable(Capability.NOMINAL_CLASS);  // only nominal classes allowed
```

## Relevant classes
* `weka.core.Capabilities`
* `weka.core.CapabilitiesHandler`
* `weka.core.MultiInstanceCapabilitiesHandler` (for multi-instance classifiers)

# Paper reference(s)
In order to make it easy to generate a bibliography of all the algorithms in Weka, the [paper references](academic/paper_references.md) located so far in the Javadoc were extracted and placed in the code.

Classes that are based on some technical paper should implement the `TechnicalInformationHandler` interface and return a customized `TechnicalInformation` instance. The format used is based on [BibTeX](http://en.wikipedia.org/wiki/BibTeX) and the `TechnicalInformation` class can either return a plain text string via the `toString()` method or a real [BibTeX](http://en.wikipedia.org/wiki/BibTeX) entry via the `toBibTex()` method. This two methods are then used to automatically update the Javadoc (see [Javadoc](writing_classifier.md#javadoc) further down) of a class.

Relevant classes:

* `weka.core.TechnicalInformation`
* `weka.core.TechnicalInformationHandler`


## Javadoc
Open-source software is only as good as its documentation. Hence, correct and up-to-date documentation is vital. So far most of the Javadoc was maintained manually, which made it hard to maintain, e.g., as soon as new options were added the Javadoc had to be changed accordingly, too. And that normally in several places:

* Class description
* `setOptions(String[])` method

Over the time the documentation got out of sync, which made it frustrating determining what options were really relevant and active. Since a lot of the documentation is already available in the code itself, the next logical step was to automate the Javadoc generation as much as possible. In the following you will see how to structure your Javadoc to reduce maintainance. For this purpose special comment tags are used, where the content in between will be replaced automatically by the classes listed below in the *Relevant classes* section.

The indentation of the generated Javadoc depends on the indentation of the `&lt;` of the starting comment tag.

This general layout order should be used for all classes:

* **class description** Javadoc

    1. globalinfo
    2. bibtex - *if available*
    3. commandline options

* **setOptions** Javadoc

    1. commandline options

## General 
The general description for all classes displayed in the GenericObjectEditor was already in place, with the following method:

```java
 globalInfo()
```

The return value can be placed in the Javadoc, surrounded by the following comment tags:

```
 <!-- globalinfo-start -->
 will be automatically replaced
 <!-- globalinfo-end -->
```

## Paper reference(s)
If available, the paper reference should also be listed in the Javadoc. Since the `globalInfo()` method should return a short version of the reference, it is sufficient to list the full [BibTeX](http://en.wikipedia.org/wiki/BibTeX) documentation:

```
 <!-- technical-bibtex-start -->
 will be automatically replaced
 <!-- technical-bibtex-end -->
```

In case it is necessary to list the short, plain text version, too, one can use the following tags:

```
 <!-- technical-plaintext-start -->
 will be automatically replaced
 <!-- technical-plaintext-end -->
```

## Options
To place the commandline options, use the following comment tags:

```
 <!-- options-start -->
 will be automatically replaced
 <!-- options-end -->
```

## Relevant classes
* `weka.core.AllJavadoc` - executes all Javadoc-producing classes
* `weka.core.GlobalInfoJavadoc` - updates the globalInfo tags
* `weka.core.OptionHandlerJavadoc` - updates the option tags
* `weka.core.TechnicalInformationHandlerJavadoc` - updates the technical tags (plain text and [BibTeX](http://en.wikipedia.org/wiki/BibTeX))

# Integration
After finishing the coding stage, it's time to integrate your classifier in the Weka framework, i.e., to make it available in the Explorer, Experimenter, etc.

The [GenericObjectEditor](generic_object_editor.md) article shows you how to tell Weka where to find your classifier and therefore displaying it in the *GenericObjectEditor*.

# Revisions
Classifiers also implement the `weka.core.RevisionHandler` interface. This provides the functionality of obtaining the [Subversion](subversion.md) revision from within Java. Classifiers that are not part of the official Weka distribution will have to implement the method `getRevision()` as follows, which will return a dummy revision of *1.0*:

```java
  /**
   * Returns the revision string.
   * 
   * @return		the revision
   */
  public String getRevision() {
    return RevisionUtils.extract("$Revision: 1.0 $");
  }
```

# Testing
Weka provides already a test framework to ensure the basic functionality of a classifier. It is essential for the classifier to pass these tests.

## Commandline test
## General
Use the CheckClassifier class to test your classifier from commandline:

```
 weka.classifiers.CheckClassifier -W classname [-- additional parameters]
```

Only the following tests may have "no" as result, the others must have a "no (OK error message)" or "yes":

* options
* updateable classifier
* weighted instances classifier
* multi-instance classifier

## Option handling
Additionally, check the **option handling** of your classifier with the following tool from commandline:

```
 weka.core.CheckOptionHandler -W classname [-- additional parameters]
```

All tests need to return *yes*.

## GenericObjectEditor
The `CheckGOE` class checks whether all the properties available in the GUI have a tooltip accompanying them and whether the `globalInfo()` method is declared:

```
 weka.core.CheckGOE -W classname [-- additional parameters]
```

All tests, once again, need to return *yes*.

## Source code
Classifiers that implement the `weka.classifiers.Sourcable` interface can output Java code of their model. In order to check the generated code, one should not only compile the code, but also test it with the following test class:

```
 weka.classifiers.CheckSource
```

This class takes the original Weka classifier, the generated code and the dataset used for generating the source code as parameters. It builds the Weka classifier on the dataset and compares the predictions, the ones from the Weka classifier and the ones from the generated source code, whether they are the same.

Here's an example call for `weka.classifiers.trees.Id3` and the generated class `weka.classifiers.WekaWrapper` (it wraps the actual generated code in a pseudo-classifier):

```bash
 java weka.classifiers.CheckSource \
    -W "weka.classifiers.trees.Id3" \
    -S weka.classifiers.WekaWrapper \
    -t data.arff \
    -c last
```

It needs to return *Tests OK!*.

## Unit tests
In order to make sure that your classifier applies to the Weka criteria, you should add your classifier to the [junit](http://www.junit.org/) unit test framework, i.e., by creating a Test class derived from `AbstractClassifierTest`. This class uses the `CheckClassifier`, `CheckOptionHandler` and `CheckGOE` class to run a battery of tests.

How to check out the unit test framework, you can find [here](subversion.md#junit).

# See also
* [GenericObjectEditor](generic_object_editor.md)
* [Paper References](academic/paper_references.md)
* [Writing your own Classifier Article](writing_classifier_article.md)

# Links
* [Build_classifier.pdf](files/Build_classifier.pdf) - MindMap for implementing a new classifier
* Weka API ([stable](http://weka.sourceforge.net/doc/), [developer](http://weka.sourceforge.net/doc.dev))
* [Freemind](http://freemind.sourceforge.net/)
* [junit](http://www.junit.org/)

