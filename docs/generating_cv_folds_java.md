This article describes how to generate train/test splits for [cross-validation](http://en.wikipedia.org/wiki/Cross-validation) using the Weka API directly. 

The following variables are given:

```java
 Instances data =  ...;   // contains the full dataset we wann create train/test sets from
 int seed = ...;          // the seed for randomizing the data
 int folds = ...;         // the number of folds to generate, >=2
```

# Randomize the data
First, randomize your data:

```java
 Random rand = new Random(seed);   // create seeded number generator
 randData = new Instances(data);   // create copy of original data
 randData.randomize(rand);         // randomize data with number generator
```

In case your data has a nominal class and you wanna perform stratified cross-validation:

```java
 randData.stratify(folds);
```

# Generate the folds

# Single run 
Next thing that we have to do is creating the train and the test set:

```java
 for (int n = 0; n < folds; n++) {
   Instances train = randData.trainCV(folds, n, rand);
   Instances test = randData.testCV(folds, n);
 
   // further processing, classification, etc.
   ...
 }
```

**Note:**

* the above code is used by the `weka.filters.supervised.instance.StratifiedRemoveFolds` filter
* the `weka.classifiers.Evaluation` class and the Explorer/Experimenter would use this method for obtaining the train set:
[[code format="java"]]
 Instances train = randData.trainCV(folds, n, rand);
[[code]]

## Multiple runs
The example above only performs one run of a cross-validation. In case you want to run 10 runs of 10-fold cross-validation, use the following loop:

```java
 Instances data = ...;  // our dataset again, obtained from somewhere
 int runs = 10;
 for (int i = 0; i < runs; i++) {
   seed = i+1;  // every run gets a new, but defined seed value
 
   // see: randomize the data
   ...
 
   // see: generate the folds
   ...
 }
```

# See also
* [Use Weka in your Java code](use_weka_in_your_java_code.md) - for general use of the Weka API

# Downloads
* [CrossValidationSingleRun.java](files/CrossValidationSingleRun.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/classifiers/CrossValidationSingleRun.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/classifiers/CrossValidationSingleRun.java)) - simulates a single run of 10-fold cross-validation
* [CrossValidationSingleRunVariant.java](files/CrossValidationSingleRunVariant.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/classifiers/CrossValidationSingleRunVariant.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/classifiers/CrossValidationSingleRunVariant.java)) - simulates a single run of 10-fold cross-validation, but outputs the confusion matrices for each single train/test pair as well.
* [CrossValidationMultipleRuns.java](files/CrossValidationMultipleRuns.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/classifiers/CrossValidationMultipleRuns.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/classifiers/CrossValidationMultipleRuns.java)) - simulates 10 runs of 10-fold cross-validation
* [CrossValidationAddPrediction.java](files/CrossValidationAddPrediction.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/classifiers/CrossValidationAddPrediction.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/classifiers/CrossValidationAddPrediction.java)) - simulates a single run of 10-fold cross-validation, but also adds the classification/distribution/error flag to the test data (uses the `AddClassification` filter)

