The most common components you might want to use are
* *Instances* - your data
* *Filter* - for preprocessing the data
* *Classifier/Clusterer* - built on the processed data
* *Evaluating* - how good is the classifier/clusterer?
* *Attribute selection* - removing irrelevant attributes from your data

The following sections explain how to use them in your own code. A link to an **example class** can be found at the end of this page, under the [Links](#links) section. The classifiers and filters always list their options in the Javadoc API ([stable](http://weka.sourceforge.net/doc.stable/), [developer](http://weka.sourceforge.net/doc.dev/) version) specification.

A comprehensive source of information is the chapter *Using the API* of the Weka manual.

# Instances 
## Datasets
The `DataSource` class is not limited to ARFF files. It can also read CSV files and other formats (basically all file formats that Weka can import via its converters; it uses the file extension to determine the associated loader).
```java
 import weka.core.converters.ConverterUtils.DataSource;
 ...
 DataSource source = new DataSource("/some/where/data.arff");
 Instances data = source.getDataSet();
 // setting class attribute if the data format does not provide this information
 // For example, the XRFF format saves the class attribute information as well
 if (data.classIndex() == -1)
   data.setClassIndex(data.numAttributes() - 1);
```

## Database
Reading from [Databases](databases.md) is slightly more complicated, but still very easy. First, you'll have to modify your [DatabaseUtils.props](databases#configuration-files) file to reflect your database connection. Suppose you want to connect to a [MySQL](http://wwww.mysql.com/) server that is running on the local machine on the default port `3306`. The MySQL JDBC driver is called [Connector/J](http://dev.mysql.com/downloads/connector/j/). (The driver class is `org.gjt.mm.mysql.Driver`.) The database where your target data resides is called `some_database`. Since you're only reading, you can use the default user `nobody` without a password. Your props file must contain the following lines:

```ini
 jdbcDriver=org.gjt.mm.mysql.Driver
 jdbcURL=jdbc:mysql://localhost:3306/some_database
```

Secondly, your Java code needs to look like this to load the data from the database:

```java
 import weka.core.Instances;
 import weka.experiment.InstanceQuery;
 ...
 InstanceQuery query = new InstanceQuery();
 query.setUsername("nobody");
 query.setPassword("");
 query.setQuery("select * from whatsoever");
 // You can declare that your data set is sparse
 // query.setSparseData(true);
 Instances data = query.retrieveInstances();
```

**Notes:**
* Don't forget to add the JDBC driver to your `CLASSPATH`.
* For MS Access, you must use the JDBC-ODBC-bridge that is part of a JDK. The [[Windows databases]] article explains how to do this.
* InstanceQuery automatically converts VARCHAR database columns to NOMINAL attributes, and long TEXT database columns to STRING attributes. So if you use InstanceQuery to do text mining against text that appears in a VARCHAR column, Weka will regard such text as nominal values. Thus it will fail to tokenize and mine that text. Use the `NominalToString` or `StringToNominal` filter (package `weka.filters.unsupervised.attribute`) to convert the attributes into the correct type.

# Option handling
Weka schemes that implement the `weka.core.OptionHandler` interface, such as classifiers, clusterers, and filters, offer the following methods for setting and retrieving options:

* `void setOptions(String[] options)`
* `String[] getOptions()`

There are several ways of setting the options:

* Manually creating a String array:

```java
   String[] options = new String[2];
   options[0] = "-R";
   options[1] = "1";
```

* Using a single command-line string and using the `splitOptions` method of the `weka.core.Utils` class to turn it into an array:

```java
    String[] options = weka.core.Utils.splitOptions("-R 1");
```

* Using the [OptionsToCode.java](files/OptionsToCode.java) class to automatically turn a command line into code. Especially handy if the command line contains nested classes that have their own options, such as kernels for SMO:

```bash
  java OptionsToCode weka.classifiers.functions.SMO
```

  will generate output like this:

```java
 // create new instance of scheme
 weka.classifiers.functions.SMO scheme = new weka.classifiers.functions.SMO();
 // set options
 scheme.setOptions(weka.core.Utils.splitOptions("-C 1.0 -L 0.0010 -P 1.0E-12 -N 0 -V -1 -W 1 -K \"weka.classifiers.functions.supportVector.PolyKernel -C 250007 -E 1.0\""));
```

Also, the [OptionTree.java](files/OptionTree.java) tool allows you to view a nested options string, e.g., used at the command line, as a tree. This can help you spot nesting errors.


# Filter 
A filter has two different properties:

* *supervised* or *unsupervised*
  either takes the class attribute into account or not

* *attribute*- or *instance*-based
  e.g., removing a certain attribute or removing instances that meet a certain condition

Most filters implement the `OptionHandler` interface, which means you can set the options via a String array, rather than setting them each manually via `set`-methods.
For example, if you want to remove the *first* attribute of a dataset, you need this filter

```java
 weka.filters.unsupervised.attribute.Remove
```

with this option

```
 -R 1
```

If you have an `Instances` object, called `data`, you can create and apply the filter like this:

```java
 import weka.core.Instances;
 import weka.filters.Filter;
 import weka.filters.unsupervised.attribute.Remove;
 ...
 String[] options = new String[2];
 options[0] = "-R";                                    // "range"
 options[1] = "1";                                     // first attribute
 Remove remove = new Remove();                         // new instance of filter
 remove.setOptions(options);                           // set options
 remove.setInputFormat(data);                          // inform filter about dataset **AFTER** setting options
 Instances newData = Filter.useFilter(data, remove);   // apply filter
```

## Filtering on-the-fly
The [FilteredClassifer](http://weka.sourceforge.net/doc.dev/weka/classifiers/meta/FilteredClassifier.html) meta-classifier is an easy way of filtering data on the fly. It removes the necessity of filtering the data before the classifier can be trained. Also, the data need not be passed through the trained filter again at prediction time. The following is an example of using this meta-classifier with the `Remove` filter and `J48` for getting rid of a numeric ID attribute in the data:

```java
 import weka.classifiers.meta.FilteredClassifier;
 import weka.classifiers.trees.J48;
 import weka.filters.unsupervised.attribute.Remove;
 ...
 Instances train = ...         // from somewhere
 Instances test = ...          // from somewhere
 // filter
 Remove rm = new Remove();
 rm.setAttributeIndices("1");  // remove 1st attribute
 // classifier
 J48 j48 = new J48();
 j48.setUnpruned(true);        // using an unpruned J48
 // meta-classifier
 FilteredClassifier fc = new FilteredClassifier();
 fc.setFilter(rm);
 fc.setClassifier(j48);
 // train and make predictions
 fc.buildClassifier(train);
 for (int i = 0; i < test.numInstances(); i++) {
   double pred = fc.classifyInstance(test.instance(i));
   System.out.print("ID: " + test.instance(i).value(0));
   System.out.print(", actual: " + test.classAttribute().value((int) test.instance(i).classValue()));
   System.out.println(", predicted: " + test.classAttribute().value((int) pred));
 }
```

Other handy meta-schemes in Weka:

* [weka.clusterers.FilteredClusterer](http://weka.sourceforge.net/doc.dev/weka/clusterers/FilteredClusterer.html)
* [weka.assocations.FilteredAssociator](http://weka.sourceforge.net/doc.dev/weka/associations/FilteredAssociator.html)


## Batch filtering
On the command line, you can enable a second input/output pair (via `-r` and `-s`) with the `-b` option, in order to process the second file with the same filter setup as the first one. Necessary, if you're using attribute selection or standardization - otherwise you end up with incompatible datasets. This is done fairly easy, since one initializes the filter only once with the `setInputFormat(Instances)` method, namely with the training set, and then applies the filter subsequently to the training set *and* the test set. The following example shows how to apply the `Standardize` filter to a train and a test set.

```java
 Instances train = ...   // from somewhere
 Instances test = ...    // from somewhere
 Standardize filter = new Standardize();
 filter.setInputFormat(train);  // initializing the filter once with training set
 Instances newTrain = Filter.useFilter(train, filter);  // configures the Filter based on train instances and returns filtered instances
 Instances newTest = Filter.useFilter(test, filter);    // create new test set
```

## Calling conventions
The `setInputFormat(Instances)` method **always** has to be the last call before the filter is applied, e.g., with `Filter.useFilter(Instances,Filter)`. *Why?* First, it is the convention for using filters and, secondly, lots of filters generate the header of the output format in the `setInputFormat(Instances)` method with the currently set options (setting otpions *after* this call doesn't have any effect any more).


# Classification
The necessary classes can be found in this package:
```
 weka.classifiers
```

## Building a Classifier
### Batch
A Weka classifier is rather simple to train on a given dataset. E.g., we can train an unpruned C4.5 tree algorithm on a given dataset *data*. The training is done via the `buildClassifier(Instances)` method.

```java
 import weka.classifiers.trees.J48;
 ...
 String[] options = new String[1];
 options[0] = "-U";            // unpruned tree
 J48 tree = new J48();         // new instance of tree
 tree.setOptions(options);     // set the options
 tree.buildClassifier(data);   // build classifier
```

### Incremental
Classifiers implementing the `weka.classifiers.UpdateableClassifier` interface can be trained incrementally. This conserves memory, since the data doesn't have to be loaded into memory all at once. See the Javadoc of this interface to see what classifiers are implementing it.

The actual process of training an incremental classifier is fairly simple:

* Call `buildClassifier(Instances)` with the structure of the dataset (may or may not contain any actual data rows).
* Subsequently call the `updateClassifier(Instance)` method to feed the classifier new `weka.core.Instance` objects, one by one.

Here is an example using data from a `weka.core.converters.ArffLoader` to train `weka.classifiers.bayes.NaiveBayesUpdateable`:
```java
 // load data
 ArffLoader loader = new ArffLoader();
 loader.setFile(new File("/some/where/data.arff"));
 Instances structure = loader.getStructure();
 structure.setClassIndex(structure.numAttributes() - 1);

 // train NaiveBayes
 NaiveBayesUpdateable nb = new NaiveBayesUpdateable();
 nb.buildClassifier(structure);
 Instance current;
 while ((current = loader.getNextInstance(structure)) != null)
   nb.updateClassifier(current);
```

A working example is [IncrementalClassifier.java](files/IncrementalClassifier.java).

## Evaluating
### Cross-validation
If you only have a training set and no test you might want to evaluate the classifier by using 10 times 10-fold cross-validation. This can be easily done via the `Evaluation` class. Here we *seed* the random selection of our folds for the CV with *1*. Check out the `Evaluation` class for more information about the statistics it produces.

```java
 import weka.classifiers.Evaluation;
 import java.util.Random;
 ...
 Evaluation eval = new Evaluation(newData);
 eval.crossValidateModel(tree, newData, 10, new Random(1));
```

**Note:** The classifier (in our example *tree*) should not be trained when handed over to the `crossValidateModel` method. **Why?** If the classifier does not abide to the Weka convention that a classifier must be re-initialized every time the `buildClassifier` method is called (in other words: subsequent calls to the `buildClassifier` method always return the same results), you will get inconsistent and worthless results. The `crossValidateModel` takes care of training and evaluating the classifier. (It creates a copy of the original classifier that you hand over to the `crossValidateModel` for each run of the cross-validation.)

### Train/test set
In case you have a dedicated test set, you can train the classifier and then evaluate it on this test set. In the following example, a J48 is instantiated, trained and then evaluated. Some statistics are printed to `stdout`:

```java
 import weka.core.Instances;
 import weka.classifiers.Evaluation;
 import weka.classifiers.trees.J48;
 ...
 Instances train = ...   // from somewhere
 Instances test = ...    // from somewhere
 // train classifier
 Classifier cls = new J48();
 cls.buildClassifier(train);
 // evaluate classifier and print some statistics
 Evaluation eval = new Evaluation(train);
 eval.evaluateModel(cls, test);
 System.out.println(eval.toSummaryString("\nResults\n======\n", false));
```

### Statistics
Some methods for retrieving the results from the evaluation:

* nominal class

    * `correct()` - number of correctly classified instances (see also `incorrect()`)
    * `pctCorrect()` - percentage of correctly classified instances (see also `pctIncorrect()`)
    * `kappa()` - Kappa statistics

* numeric class
 
    * `correlationCoefficient()` - correlation coefficient

* general

    * `meanAbsoluteError()` - the mean absolute error
    * `rootMeanSquaredError()` - the root mean squared error
    * `unclassified()` - number of unclassified instances
    * `pctUnclassified()` - percentage of unclassified instances

If you want to have the exact same behavior as from the command line, use this call:

```java
 import weka.classifiers.trees.J48;
 import weka.classifiers.Evaluation;
 ...
 String[] options = new String[2];
 options[0] = "-t";
 options[1] = "/some/where/somefile.arff";
 System.out.println(Evaluation.evaluateModel(new J48(), options));
```

### ROC curves/AUC
You can also generate ROC curves/AUC with the predictions Weka recorded during testing. You can access these predictions via the `predictions()` method of the `Evaluation` class. See the [[Generating ROC curve]] article for a full example of how to generate ROC curves.

## Classifying instances
In case you have an unlabeled dataset that you want to classify with your newly trained classifier, you can use the following code snippet. It loads the file `/some/where/unlabeled.arff`, uses the previously built classifier `tree` to label the instances, and saves the labeled data as `/some/where/labeled.arff`.

```java
 import java.io.BufferedReader;
 import java.io.BufferedWriter;
 import java.io.FileReader;
 import java.io.FileWriter;
 import weka.core.Instances;
 ...
 // load unlabeled data
 Instances unlabeled = new Instances(
                         new BufferedReader(
                           new FileReader("/some/where/unlabeled.arff")));

 // set class attribute
 unlabeled.setClassIndex(unlabeled.numAttributes() - 1);

 // create copy
 Instances labeled = new Instances(unlabeled);

 // label instances
 for (int i = 0; i < unlabeled.numInstances(); i++) {
   double clsLabel = tree.classifyInstance(unlabeled.instance(i));
   labeled.instance(i).setClassValue(clsLabel);
 }
 // save labeled data
 BufferedWriter writer = new BufferedWriter(
                           new FileWriter("/some/where/labeled.arff"));
 writer.write(labeled.toString());
 writer.newLine();
 writer.flush();
 writer.close();
```

**Note on nominal classes:**

* If you're interested in the distribution over all the classes, use the method `distributionForInstance(Instance)`. This method returns a double array with the probability for each class.
* The returned double value from `classifyInstance` (or the index in the array returned by `distributionForInstance`) is just the index for the string values in the attribute. That is, if you want the string representation for the class label returned above `clsLabel`, then you can print it like this:

```java
System.out.println(clsLabel + " -> " + unlabeled.classAttribute().value((int) clsLabel));
```

# Clustering
Clustering is similar to classification. The necessary classes can be found in this package:
```
 weka.clusterers
```

## Building a Clusterer
### Batch
A clusterer is built in much the same way as a classifier, but the `buildClusterer(Instances)` method instead of `buildClassifier(Instances)`. The following code snippet shows how to build an `EM` clusterer with a maximum of `100` iterations.
```java
 import weka.clusterers.EM;
 ...
 String[] options = new String[2];
 options[0] = "-I";                 // max. iterations
 options[1] = "100";
 EM clusterer = new EM();   // new instance of clusterer
 clusterer.setOptions(options);     // set the options
 clusterer.buildClusterer(data);    // build the clusterer
```

### Incremental
Clusterers implementing the `weka.clusterers.UpdateableClusterer` interface can be trained incrementally. This conserves memory, since the data doesn't have to be loaded into memory all at once. See the Javadoc for this interface to see which clusterers implement it.

The actual process of training an incremental clusterer is fairly simple:

* Call `buildClusterer(Instances)` with the structure of the dataset (may or may not contain any actual data rows).
* Subsequently call the `updateClusterer(Instance)` method to feed the clusterer new `weka.core.Instance` objects, one by one.
* Call `updateFinished()` after all Instance objects have been processed, for the clusterer to perform additional computations.

Here is an example using data from a `weka.core.converters.ArffLoader` to train `weka.clusterers.Cobweb`:

```java
 // load data
 ArffLoader loader = new ArffLoader();
 loader.setFile(new File("/some/where/data.arff"));
 Instances structure = loader.getStructure();

 // train Cobweb
 Cobweb cw = new Cobweb();
 cw.buildClusterer(structure);
 Instance current;
 while ((current = loader.getNextInstance(structure)) != null)
   cw.updateClusterer(current);
 cw.updateFinished();
```

A working example is [IncrementalClusterer.java](files/IncrementalClusterer.java).

## Evaluating
For evaluating a clusterer, you can use the `ClusterEvaluation` class. In this example, the number of clusters found is written to output:

```java
 import weka.clusterers.ClusterEvaluation;
 import weka.clusterers.Clusterer;
 ...
 ClusterEvaluation eval = new ClusterEvaluation();
 Clusterer clusterer = new EM();                                 // new clusterer instance, default options
 clusterer.buildClusterer(data);                                 // build clusterer
 eval.setClusterer(clusterer);                                   // the cluster to evaluate
 eval.evaluateClusterer(newData);                                // data to evaluate the clusterer on
 System.out.println("# of clusters: " + eval.getNumClusters());  // output # of clusters
```

Or, in the case of [DensityBasedClusterer](http://weka.sourceforge.net/doc/weka/clusterers/DensityBasedClusterer.html), you can cross-validate the clusterer (Note: with [MakeDensityBasedClusterer](http://weka.sourceforge.net/doc/weka/clusterers/MakeDensityBasedClusterer.html) you can turn any clusterer into a density-based one):

```java
 import weka.clusterers.ClusterEvaluation;
 import weka.clusterers.DensityBasedClusterer;
 import weka.core.Instances;
 import java.util.Random;
 ...
 Instances data = ...                                     // from somewhere
 DensityBasedClusterer clusterer = new ...                // the clusterer to evaluate
 double logLikelyhood =
    ClusterEvaluation.crossValidateModel(                 // cross-validate
    clusterer, data, 10,                                  // with 10 folds
    new Random(1));                                       // and random number generator with seed 1
```

Or, if you want the same behavior/print-out from command line, use this call:

```java
 import weka.clusterers.EM;
 import weka.clusterers.ClusterEvaluation;
 ...
 String[] options = new String[2];
 options[0] = "-t";
 options[1] = "/some/where/somefile.arff";
 System.out.println(ClusterEvaluation.evaluateClusterer(new EM(), options));
```

## Clustering instances
The only difference with regard to classification is the method name. Instead of `classifyInstance(Instance)`, it is now `clusterInstance(Instance)`. The method for obtaining the distribution is still the same, i.e., `distributionForInstance(Instance)`.

## Classes to clusters evaluation
If your data contains a class attribute and you want to check how well the generated clusters fit the classes, you can perform a so-called *classes to clusters* evaluation. The Weka Explorer offers this functionality, and it's quite easy to implement. These are the necessary steps (complete source code: [ClassesToClusters.java](files/ClassesToClusters.java)):

* load the data and set the class attribute

```java
 Instances data = new Instances(new BufferedReader(new FileReader("/some/where/file.arff")));
 data.setClassIndex(data.numAttributes() - 1);
```

* generate the *class-less* data to train the clusterer with

```java
 weka.filters.unsupervised.attribute.Remove filter = new weka.filters.unsupervised.attribute.Remove();
 filter.setAttributeIndices("" + (data.classIndex() + 1));
 filter.setInputFormat(data);
 Instances dataClusterer = Filter.useFilter(data, filter);
```

* train the clusterer, e.g., `EM`

```java
 EM clusterer = new EM();
 // set further options for EM, if necessary...
 clusterer.buildClusterer(dataClusterer);
```

* evaluate the clusterer with the data still containing the class attribute
```java
 ClusterEvaluation eval = new ClusterEvaluation();
 eval.setClusterer(clusterer);
 eval.evaluateClusterer(data);
```

* print the results of the evaluation to *stdout*

```java
 System.out.println(eval.clusterResultsToString());
```

# Attribute selection
There is no real need to use the attribute selection classes directly in your own code, since there are already a meta-classifier and a filter available for applying attribute selection, but the low-level approach is still listed for the sake of completeness. The following examples all use `CfsSubsetEval` and `GreedyStepwise` (backwards). The code listed below is taken from the [AttributeSelectionTest.java](files/AttributeSelectionTest.java).

## Meta-Classifier
The following meta-classifier performs a preprocessing step of attribute selection before the data gets presented to the base classifier (in the example here, this is `J48`).

```java
  Instances data = ...  // from somewhere
  AttributeSelectedClassifier classifier = new AttributeSelectedClassifier();
  CfsSubsetEval eval = new CfsSubsetEval();
  GreedyStepwise search = new GreedyStepwise();
  search.setSearchBackwards(true);
  J48 base = new J48();
  classifier.setClassifier(base);
  classifier.setEvaluator(eval);
  classifier.setSearch(search);
  // 10-fold cross-validation
  Evaluation evaluation = new Evaluation(data);
  evaluation.crossValidateModel(classifier, data, 10, new Random(1));
  System.out.println(evaluation.toSummaryString());
```

## Filter
The filter approach is straightforward: after setting up the filter, one just filters the data through the filter and obtains the reduced dataset.

```java
  Instances data = ...  // from somewhere
  AttributeSelection filter = new AttributeSelection();  // package weka.filters.supervised.attribute!
  CfsSubsetEval eval = new CfsSubsetEval();
  GreedyStepwise search = new GreedyStepwise();
  search.setSearchBackwards(true);
  filter.setEvaluator(eval);
  filter.setSearch(search);
  filter.setInputFormat(data);
  // generate new data
  Instances newData = Filter.useFilter(data, filter);
  System.out.println(newData);
```

## Low-level
If neither the meta-classifier nor filter approach is suitable for your purposes, you can use the attribute selection classes themselves.

```java
  Instances data = ...  // from somewhere
  AttributeSelection attsel = new AttributeSelection();  // package weka.attributeSelection!
  CfsSubsetEval eval = new CfsSubsetEval();
  GreedyStepwise search = new GreedyStepwise();
  search.setSearchBackwards(true);
  attsel.setEvaluator(eval);
  attsel.setSearch(search);
  attsel.SelectAttributes(data);
  // obtain the attribute indices that were selected
  int[] indices = attsel.selectedAttributes();
  System.out.println(Utils.arrayToString(indices));
```

# Note on randomization
Most machine learning schemes, like classifiers and clusterers, are susceptible to the ordering of the data. Using a different seed for randomizing the data will most likely produce a different result. For example, the Explorer, or a classifier/clusterer run from the command line, uses only a seeded `java.util.Random` number generator, whereas the `weka.core.Instances.getRandomNumberGenerator(int)` (which the [WekaDemo.java](files/WekaDemo.java) uses) also takes the data into account for seeding. Unless one runs 10-fold cross-validation 10 times and averages the results, one will most likely get different results.

# See also
* [Weka Examples](weka_examples.md) - pointer to collection of example classes
* [Databases](databases.md) - for more information about using databases in Weka (includes ODBC, e.g., for MS Access)
* [[weka_experiment_DatabaseUtils.props|weka/experiment/DatabaseUtils.props]] - the database setup file
* [Generating cross-validation folds (Java approach)](generating_cv_folds_java.md) - in case you want to run 10-fold cross-validation manually
* [[Generating classifier evaluation output manually]] - if you want to generate some of the evaluation statistics output manually
* [Creating Instances on-the-fly](creating_arff_file.md) - explains how to generate a `weka.core.Instances` object from scratch
* [[Save Instances to an ARFF File]] - shows how to output a dataset
* [[Using the Experiment API]]

# Examples
The following are a few sample classes for using various parts of the Weka API:

* [WekaDemo.java](files/WekaDemo.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/classifiers/WekaDemo.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/classifiers/WekaDemo.java)) - little demo class that loads data from a file, runs it through a filter and trains/evaluates a classifier

* [ClusteringDemo.java](files/ClusteringDemo.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/clusterers/ClusteringDemo.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/clusterers/ClusteringDemo.java)) - a basic example for using the clusterer API

* [ClassesToClusters.java](files/ClassesToClusters.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/clusterers/ClassesToClusters.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/clusterers/ClassesToClusters.java)) - performs a *classes to clusters* evaluation like in the Explorer

* [AttributeSelectionTest.java](files/AttributeSelectionTest.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/attributeSelection/AttributeSelectionTest.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/attributeSelection/AttributeSelectionTest.java)) - example code for using the attribute selection API

* [M5PExample.java](files/M5PExample.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/classifiers/M5PExample.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/classifiers/M5PExample.java)) - example using M5P to obtain data from database, train model, serialize it to a file, and use this serialized model to make predictions again.

* [OptionsToCode.java](files/OptionsToCode.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/core/OptionsToCode.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/core/OptionsToCode.java)) - turns a Weka command line for a scheme with options into Java code, correctly escaping quotes and backslashes.

* [OptionTree.java](files/OptionTree.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/gui/OptionTree.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/gui/OptionTree.java)) - displays nested Weka options as tree.

* [IncrementalClassifier.java](files/IncrementalClassifier.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/classifiers/IncrementalClassifier.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/classifiers/IncrementalClassifier.java)) - Example class for how to train an incremental classifier (in this case, `weka.classifiers.bayes.NaiveBayesUpdateable`).

* [IncrementalClusterer.java](files/IncrementalClusterer.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/clusterers/IncrementalClusterer.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/clusterers/IncrementalClusterer.java)) - Example class for how to train an incremental clusterer (in this case, `weka.clusterers.Cobweb`).

# Links
* Weka API
    * [Stable version](http://weka.sourceforge.net/doc.stable/)
    * [Developer version]([http://weka.sourceforge.net/doc.dev/)
