

In the following some code snippets that explain how to generate the output Weka generates when one runs a classifier from the commandline. When referring to the `Evaluation` class, the [weka.classifiers.Evaluation](http://weka.sourceforge.net/doc.dev/weka/classifiers/Evaluation.html) class is meant. This article provides only a quick overview, for more details, please see the [Javadoc](http://weka.sourceforge.net/doc.dev/) of the [Evaluation](http://weka.sourceforge.net/doc.dev/weka/classifiers/Evaluation.html) class.

# Model
A classifier's model, if that classifier supports the output of it, can be simply output by using the `toString()` method after it got trained:

```java
 Instances data = ... // from somewhere
 Classifier cls = new weka.classifiers.trees.J48();
 cls.buildClassifier(data);
 System.out.println(cls);
```
**NB:** Weka always outputs the model based on the *full* training set (provided with the option `-t`), no matter whether cross-validation is used or a designated test set (via `-T`). The 10 models generated during a 10-fold cross-validation run are never output. If you want to output these models you have to simulate the `crossValidateModel` method yourself, use the KnowledgeFlow (see article [Displaying results of cross-validation folds](visualization/displaying_results_of_cross_validation_folds.md)).

# Statistics
The statistics, also called the summary of an evaluation, can be be generated via the `toSummaryString` methods. Here is an example of the summary from a cross-validated J48:

```java
 Classifier cls = new J48();
 Evaluation eval = new Evaluation(data);
 Random rand = new Random(1);  // using seed = 1
 int folds = 10;
 eval.crossValidateModel(cls, data, folds, rand);
 System.out.println(eval.toSummaryString());
```

# Detailed class statistics
In order to generate the detailed statistics per class (on the commandline via option `-i`), one can use the `toClassDetailsString` methods. Once again a code snippet featuring a cross-validated J48:

```java
 Classifier cls = new J48();
 Evaluation eval = new Evaluation(data);
 Random rand = new Random(1);  // using seed = 1
 int folds = 10;
 eval.crossValidateModel(cls, data, folds, rand);
 System.out.println(eval.toClassDetailsString());
```

# Confusion matrix
The confusion matrix is simply output with the `toMatrixString()` or `toMatrixString(String)` method of the `Evaluation` class. In the following an example of cross-validating J48 on a dataset and outputting the confusion matrix to stdout.
```java
 Classifier cls = new J48();
 Evaluation eval = new Evaluation(data);
 Random rand = new Random(1);  // using seed = 1
 int folds = 10;
 eval.crossValidateModel(cls, data, folds, rand);
 System.out.println(eval.toMatrixString());
```

# See also
* [Use Weka in your Java code](use_weka_in_your_java_code.md) - general overview of the Weka API
