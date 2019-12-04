[WEKA](http://www.cs.waikato.ac.nz/%7eml/weka/) is a comprehensive workbench for machine learning and data mining. Its main strengths lie in the classification area, where many of the main machine learning approaches have been implemented within a clean, object-oriented Java class hierarchy. Regression, association rule mining, time series prediction, and clustering algorithms have also been implemented.

This document serves as a brief introduction to using WEKA from the command line interface. We will begin by describing [basic concepts](primer.md#basic-concepts) and ideas. Then, we will describe the [weka.filters](primer.md#weka-filters) package, which is used to transform input data, e.g., for preprocessing, transformation, feature generation and so on. Following that, we will consider some [machine learning algorithms](primer.md#classifier) that generate classification models. Afterwards, some practical examples are given.

Note that, in the doc directory of the WEKA installation directory, you can find documentation of all Java classes in WEKA. Prepare to use it since this introduction is not intended to be complete. If you want to know exactly what is going on, take a look at the source code, which can be found in weka-src.jar and can be extracted via the jar utility from the Java Development Kit.

# Basic concepts 
## Dataset

A set of data items, the dataset, is a very basic concept of machine learning. A dataset is roughly equivalent to a two-dimensional spreadsheet or database table. In WEKA, it is implemented by the [Instances](https://weka.sourceforge.io/doc.stable-3-8/weka/core/Instances.html) class. A dataset is a collection of examples, each one of class [Instance](https://weka.sourceforge.io/doc.stable-3-8/weka/core/Instance.html). Each Instance consists of a number of attributes, any of which can be nominal (= one of a predefined list of values), numeric (= a real or integer number) or a string (= an arbitrary long list of characters, enclosed in "double quotes"). WEKA also supports date attributes and relational attributes. The external representation of an Instances class is an ARFF file, which consists of a header describing the attribute types and the data as comma-separated list. Here is a short, commented example. A complete description of the ARFF file format can be found [here](formats_and_processing/arff.md).

```
% This is a toy example, the UCI weather dataset.
% Any relation to real weather is purely coincidental.
```
Comment lines at the beginning of the dataset should give an indication of its source, context and meaning.

```
@relation golfWeatherMichigan_1988/02/10_14days
```
Here we state the internal name of the dataset. Try to be as descriptive as possible.

```
@attribute outlook {sunny, overcast rainy}
@attribute windy {TRUE, FALSE}
```
Here we define two nominal attributes, *outlook* and *windy*. The former has three values: *sunny*, *overcast* and *rainy*; the latter two: 
*TRUE* and *FALSE*. Nominal values with special characters, commas or spaces are enclosed in 'single quotes'.

```
@attribute temperature numeric
@attribute humidity numeric
```
These lines define two numeric attributes.

```
@attribute play {yes, no}
```
The last attribute is the default target or class variable used for prediction. In our case it is a nominal attribute with two values, making this a binary classification problem.

```
@data
sunny,FALSE,85,85,no
sunny,TRUE,80,90,no
overcast,FALSE,83,86,yes
rainy,FALSE,70,96,yes
rainy,FALSE,68,80,yes
```
The rest of the dataset consists of the token @data, followed by comma-separated values for the attributes -- one line per example. In our case there are five examples.

Some basic statistics and validation of given ARFF files can be obtained via the main() routine of [weka.core.Instances](https://weka.sourceforge.io/doc.stable-3-8/weka/core/Instances.html):

```bash
 java weka.core.Instances data/soybean.arff
```
`weka.core` offers some other useful routines, e.g., [converters.C45Loader](https://weka.sourceforge.io/doc.stable-3-8/weka/core/converters/C45Loader.html) and [converters.CSVLoader](https://weka.sourceforge.io/doc.stable-3-8/weka/core/converters/CSVLoader.html), which can be used to convert C45 datasets and comma/tab-separated datasets respectively, e.g.:

```bash
 java weka.core.converters.CSVLoader data.csv > data.arff
 java weka.core.converters.C45Loader c45_filestem > data.arff
```

## Classifier
Any classification or regression algorithm in WEKA is derived from the abstract [Classifier](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/Classifier.html) class. Surprisingly little is needed for a basic classifier: 
a routine which generates a classifier model from a training dataset (= `buildClassifier`) and another routine which produces a classification for a given instance (= `classifyInstance`), or generates a probability distribution for all classes of the instance (= `distributionForInstance`).

A classifier model is an arbitrary complex mapping from predictor attributes to the class attribute. The specific form and creation of this mapping, or model, differs from classifier to classifier. For example, [ZeroR's](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/rules/ZeroR.html) model just consists of a single value: 
the most common class in the case of classification problems, or the median of all numeric values in case of predicting a numeric value (= regression learning). ZeroR is a trivial classifier, but it gives a lower bound on the performance of a given dataset that should be significantly improved by more complex classifiers. As such it is a reasonable test of how well the class can be predicted without considering the other attributes.

[Later](primer.md#classifiers), we will explain how to interpret the output from classifiers in detail -- for now just focus on the *Correctly Classified Instances* in the section *Stratified cross-validation* and notice how it improves from ZeroR to J48 when we use the soybean data:

```bash
 java weka.classifiers.rules.ZeroR -t soybean.arff
 java weka.classifiers.trees.J48 -t soybean.arff
```
There are various approaches to determine the performance of classifiers. It can most simply be measured by counting the proportion of correctly predicted examples in a test dataset. This value is the *classification* *accuracy*, which is also *1-ErrorRate*. Both terms are used in literature.

The simplest case for evaluation is when we use a training set and a test set which are mutually independent. This is referred to as hold-out estimate. To estimate variance in these performance estimates, hold-out estimates may be computed by repeatedly by resampling the same dataset -- i.e., randomly shuffling it and then splitting it into training and test sets with a specific proportion of the examples, collecting all estimates on the test sets and computing average and standard deviation of accuracy.

A more elaborate method is *k*-fold cross-validation. Here, a number of folds *k* is specified. The dataset is randomly shuffled and then split into *k* folds of equal size. In each iteration, one fold is used for testing and the other *k-1* folds are used for training the classifier. The test results are collected and pooled (or averaged) over all folds. This gives the cross-validation estimate of accuracy. The folds can be purely random or slightly modified to create the same class distributions in each fold as in the complete dataset. In the latter case the cross-validation is called *stratified*. Leave-one-out (loo) cross-validation signifies that *k* is equal to the number of examples. Out of necessity, loo cv has to be non-stratified, i.e., the class distributions in the test sets are not the same as those in the training data. Therefore loo CV can produce misleading results in rare cases. However it is still quite useful in dealing with small datasets since it utilizes the greatest amount of training data from the dataset.

# weka filters 
The [weka.filters](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/Filter.html) package contains Java classes that transform datasets -- by removing or adding attributes, resampling the dataset, removing examples and so on. This package offers useful support for data preprocessing, which is an important step in machine learning.

All filters offer the command-line option *-i* for specifying the input dataset, and the option *-o* for specifying the output dataset. If any of these parameters is not given, this specifies standard input resp. output for use within pipes. Other parameters are specific to each filter and can be found out via -*h*, as with any other class. The weka.filters package is organized into supervised and unsupervised filtering, both of which are again subdivided into instance and attribute filtering. We will discuss each of the four subsection separately.

## weka.filters.supervised
Classes below weka.filters.supervised in WEKA's Java class hierarchy are for supervised filtering, i.e., taking advantage of the class information. For those filters, a class must be assigned by providing the index of the class attribute via *-c*.

## attribute
[Discretize](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/supervised/attribute/Discretize.html) is used to discretize numeric attributes into nominal ones, based on the class information, via Fayyad & Irani's MDL method, or optionally with Kononeko's MDL method. Some learning schemes or classifiers can only process nominal data, e.g., [rules.Prism](https://weka.sourceforge.io/doc.packages/simpleEducationalLearningSchemes/weka/classifiers/rules/Prism.html); and in some cases discretization may also reduce learning time and help combat overfitting.
```bash
 java weka.filters.supervised.attribute.Discretize -i data/iris.arff -o iris-nom.arff -c last
 java weka.filters.supervised.attribute.Discretize -i data/cpu.arff -o cpu-classvendor-nom.arff -c first
```
[NominalToBinary](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/supervised/attribute/NominalToBinary.html) encodes all nominal attributes into binary (two-valued) attributes, which can be used to transform the dataset into a purely numeric representation, e.g., for visualization via multi-dimensional scaling.
```bash
 java weka.filters.supervised.attribute.NominalToBinary -i data/contact-lenses.arff -o contact-lenses-bin.arff -c last
```
Note that most classifiers in WEKA utilize transformation filters internally, e.g., Logistic and SMO, so you may not have to use these filters explicity.

## instance
[Resample](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/supervised/instance/Resample.html) creates a stratified subsample of the given dataset. This means that overall class distributions are approximately retained within the sample. A bias towards uniform class distribution can be specified via -*B*.
```bash
 java weka.filters.supervised.instance.Resample -i data/soybean.arff -o soybean-5%.arff -c last -Z 5
 java weka.filters.supervised.instance.Resample -i data/soybean.arff -o soybean-uniform-5%.arff -c last -Z 5 -B 1
```
[StratifiedRemoveFolds](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/supervised/instance/StratifiedRemoveFolds.html) creates stratified cross-validation folds of the given dataset. This means that per default the class distributions are approximately retained within each fold. The following example splits soybean.arff into stratified training and test datasets, the latter consisting of 25% (=1/4) of the data.
```bash
 java weka.filters.supervised.instance.StratifiedRemoveFolds -i data/soybean.arff -o soybean-train.arff \
   -c last -N 4 -F 1 -V
 java weka.filters.supervised.instance.StratifiedRemoveFolds -i data/soybean.arff -o soybean-test.arff \
   -c last -N 4 -F 1
```

## weka.filters.unsupervised
Classes below `weka.filters.unsupervised` in WEKA's Java class hierarchy are for unsupervised filtering, e.g., the non-stratified version of Resample. A class should not be assigned here.

## attribute
[StringToWordVector](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/unsupervised/attribute/StringToWordVector.html) transforms string attributes into a word vectors, e.g., creating one attribute for each word that either encodes presence or word count (*-C*) within the string. *-W* can be used to set an approximate limit on the number of words. When a class is assigned, the limit applies to each class separately. This filter is useful for text mining.

[Obfuscate](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/unsupervised/attribute/Obfuscate.html) renames the dataset name, all attribute names and nominal attribute values. This is intended for exchanging sensitive datasets without giving away restricted information.

[Remove](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/unsupervised/attribute/Remove.html) is intended for explicit deletion of attributes from a dataset, e.g. for removing attributes of the iris dataset:

```bash
 java weka.filters.unsupervised.attribute.Remove -R 1-2 -i data/iris.arff -o iris-simplified.arff
 java weka.filters.unsupervised.attribute.Remove -V -R 3-last -i data/iris.arff -o iris-simplified.arff
```

## instance
[Resample](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/unsupervised/instance/Resample.html) creates a non-stratified subsample of the given dataset. It performs random sampling without regard to the class information. Otherwise it is equivalent to its supervised variant.
```bash
 java weka.filters.unsupervised.instance.Resample -i data/soybean.arff -o soybean-5%.arff -Z 5
```
[RemoveFolds](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/unsupervised/instance/RemoveFolds.html) creates cross-validation folds of the given dataset. The class distributions are not retained. The following example splits soybean.arff into training and test datasets, the latter consisting of 25% (=1/4) of the data.
```bash
 java weka.filters.unsupervised.instance.RemoveFolds -i data/soybean.arff -o soybean-train.arff -c last -N 4 -F 1 -V
 java weka.filters.unsupervised.instance.RemoveFolds -i data/soybean.arff -o soybean-test.arff -c last -N 4 -F 1
```
[RemoveWithValues](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/unsupervised/instance/RemoveWithValues.html) filters instances according to the value of an attribute.
```bash
 java weka.filters.unsupervised.instance.RemoveWithValues -i data/soybean.arff \
   -o soybean-without_herbicide_injury.arff -V -C last -L 19
```

# weka.classifiers 
Classifiers are at the core of WEKA. There are a lot of common options for classifiers, most of which are related to evaluation purposes. We will focus on the most important ones. All others including classifier-specific parameters can be found via -*h*, as usual.

Parameter | Description
--- | --- 
*-t* | specifies the training file (ARFF format)
*-T* | specifies the test file in (ARFF format). If this parameter is missing, a crossvalidation will be performed (default: 10-fold cv) 
*-x* | This parameter determines the number of folds for the cross-validation. A cv will only be performed if -T is missing.
*-c* | As we already know from the weka.filters section, this parameter sets the class variable with a one-based index.
*-d* | The model after training can be saved via this parameter. Each classifier has a different binary format for the model, so it can only be read back by the ct same classifier on a compatible dataset. Only the model on the training set is saved, not the multiple models generated via cross-validation.
*-l* | Loads a previously saved model, usually for testing on new, previously unseen data. In that case, a compatible test file should be specified, i.e. the same ributes in the same order.
*-p <attrib_range>* | If a test file is specified, this parameter shows you the predictions and one attribute (0 for none) for all test instances.
*-o* | This parameter switches the human-readable output of the model description off. In case of support vector machines or NaiveBayes, this makes some sense unless you want to parse and visualize a lot of information.

We now give a short list of selected classifiers in WEKA:

* [`trees.J48`](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/trees/J48.html) A clone of the C4.5 decision tree learner
* [`bayes.NaiveBayes`](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/bayes/NaiveBayes.html) A Naive Bayesian learner. *-K* switches on kernel density estimation for numerical attributes which often improves performance.
* [`meta.ClassificationViaRegression`](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/meta/ClassificationViaRegression.html) -W [`functions.LinearRegression`](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/functions/LinearRegression.html) Multi-response linear regression.
* [`functions.Logistic`](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/functions/Logistic.html) Logistic Regression.
* [`functions.SMO`](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/functions/SMO.html) Support Vector Machine (linear, polynomial and RBF kernel) with Seuential Minimal Optimization Algorithm due to [Platt, 1998]. Defaults to SVM with linear kernel, *-E 5 -C 10* gives an SVM with polynomial kernel of degree 5 and lambda=10.
* [`lazy.KStar`](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/lazy/KStar.html) Instance-Based learner. *-E* sets the blend entropy automatically, which is usa`lly preferable.
* [`lazy.IBk`](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/lazy/IBk.html) Instance-Based learner with fixed neighborhood. *-K* sets the number of neighbors tou`se. *IB1* is equivalent to *IBk -K 1*
* [`rules.JRip`](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/rules/JRip.html) A clone of the RIPPER rule learner.

Based on a simple example, we will now explain the output of a typical classifier, [weka.classifiers.trees.J48](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/trees/J48.html). Consider the following call from the command line, or start the WEKA explorer and train J48 on weather.numeric.arff:

```bash
 java weka.classifiers.trees.J48 -t data/weather.numeric.arff
```

```
J48 pruned tree
------------------

outlook = sunny
|   humidity <= 75: 
yes (2.0)
|   humidity > 75: 
no (3.0)
outlook = overcast: 
yes (4.0)
outlook = rainy
|   windy = TRUE: 
no (2.0)
|   windy = FALSE: 
yes (3.0)

Number of Leaves  : 
 5

Size of the tree : 
 8
```
The first part, unless you specify *-o*, is a human-readable form of the training set model. In this case, it is a decision tree. *outlook* is at the root of the tree and determines the first decision. In case it is overcast, we'll always play golf. The numbers in (parentheses) at the end of each leaf tell us the number of examples in this leaf. If one or more leaves were not pure (= all of the same class), the number of misclassified examples would also be given, after a /slash/

```
Time taken to build model: 
0.05 seconds
Time taken to test model on training data: 
0 seconds
```
As you can see, a decision tree learns quite fast and is evaluated even faster.

```
== Error on training data ==

Correctly Classified Instance      14              100      %
Incorrectly Classified Instances    0                0      %
Kappa statistic                     1
Mean absolute error                 0
Root mean squared error             0
Relative absolute error             0      %
Root relative squared error         0      %
Total Number of Instances          14

== Detailed Accuracy By Class ==

TP Rate   FP Rate   Precision   Recall  F-Measure   Class
  1         0          1         1         1        yes
  1         0          1         1         1        no

== Confusion Matrix ==

 a b   <-- classified as
 9 0 | a = yes
 0 5 | b = no
```
This is quite boring: 
our classifier is perfect, at least on the training data -- all instances were classified correctly and all errors are zero. As is usually the case, the training set accuracy is too optimistic. The detailed accuracy by class and the confusion matrix is similarily trivial.

```
== Stratified cross-validation ==

Correctly Classified Instances      9               64.2857 %
Incorrectly Classified Instances    5               35.7143 %
Kappa statistic                     0.186
Mean absolute error                 0.2857
Root mean squared error             0.4818
Relative absolute error            60      %
Root relative squared error        97.6586 %
Total Number of Instances          14


== Detailed Accuracy By Class ==

TP Rate   FP Rate   Precision   Recall  F-Measure   Class
  0.778     0.6        0.7       0.778     0.737    yes
  0.4       0.222      0.5       0.4       0.444    no


== Confusion Matrix ==

 a b   <-- classified as
 7 2 | a = yes
 3 2 | b = no
```
The stratified cross-validation paints a more realistic picture. The accuracy is around 64%. The kappa statistic measures the agreement of prediction with the true class -- 1.0 signifies complete agreement. The error values that are shown, e.g., the root of the mean squared error, indicate the accuracy of the probability estimates that are generated by the classification model.

The confusion matrix is more commonly named *contingency table*. In our case we have two classes, and therefore a 2x2 confusion matrix, the matrix could be arbitrarily large. The number of correctly classified instances is the sum of diagonals in the matrix; all others are incorrectly classified (class "a" gets misclassified as "b" exactly twice, and class "b" gets misclassified as "a" three times).

The *True Positive (TP)* rate is the proportion of examples which were classified as class *x*, among all examples which truly have class *x*, i.e., how much of the class was captured correctly. It is equivalent to *Recall*. In the confusion matrix, this is the diagonal element divided by the sum over the relevant row, i.e., 7/(7+2)=0.778 for class *yes* and 2/(3+2)=0.4 for class *no* in our example.

The *False Positive (FP)* rate is the proportion of examples which were classified as class *x*, but belong to a different class, among all examples which are not of class *x*. In the matrix, this is the column sum of class *x* minus the diagonal element, divided by the row sums of all other classes; i.e. 3/5=0.6 for class *yes* and 2/9=0.222 for class *no*.

The *Precision* is the proportion of the examples which truly have class *x* among all those which were classified as class *x*. In the matrix, this is the diagonal element divided by the sum over the relevant column, i.e. 7/(7+3)=0.7 for class *yes* and 2/(2+2)=0.5 for class *no*.

The *F-Measure* is simply 2*Precision*Recall/(Precision+Recall), a combined measure for precision and recall.

These measures are useful for comparing classifiers. However, if more detailed information about the classifier's predictions are necessary, *-p #* outputs just the predictions for each test instance, along with a range of one-based attribute ids (0 for none). Let's look at the following example. We shall assume soybean-train.arff and soybean-test.arff have been constructed via weka.filters.supervised.instance.StratifiedRemoveFolds as in a previous example.
```java
 java weka.classifiers.bayes.NaiveBayes -K -t soybean-train.arff -T soybean-test.arff -p 0
```

```
0 diaporthe-stem-canker 0.9999672587892333 diaporthe-stem-canker
1 diaporthe-stem-canker 0.9999992614503429 diaporthe-stem-canker
2 diaporthe-stem-canker 0.999998948559035 diaporthe-stem-canker
3 diaporthe-stem-canker 0.9999998441238833 diaporthe-stem-canker
4 diaporthe-stem-canker 0.9999989997681132 diaporthe-stem-canker
5 rhizoctonia-root-rot 0.9999999395928124 rhizoctonia-root-rot
6 rhizoctonia-root-rot 0.999998912860593 rhizoctonia-root-rot
7 rhizoctonia-root-rot 0.9999994386283236 rhizoctonia-root-rot
...
```
The values in each line are separated by a single space. The fields are the zero-based test instance id, followed by the predicted class value, the confidence for the prediction (estimated probability of predicted class), and the true class. All these are correctly classified, so let's look at a few erroneous ones.

```
32 phyllosticta-leaf-spot 0.7789710144361445 brown-spot
...
39 alternarialeaf-spot 0.6403333824349896 brown-spot
...
44 phyllosticta-leaf-spot 0.893568420641914 brown-spot
...
46 alternarialeaf-spot 0.5788190397739439 brown-spot
...
73 brown-spot 0.4943768155314637 alternarialeaf-spot
...
```
In each of these cases, a misclassification occurred, mostly between classes *alternarialeaf-spot* and *brown-spot*. The confidences seem to be lower than for correct classification, so for a real-life application it may make sense to output *don't know* below a certain threshold. WEKA also outputs a trailing newline.

If we had chosen a range of attributes via *-p*, e.g., *-p first-last*, the mentioned attributes would have been output afterwards as comma-separated values, in parantheses. However, the zero-based instance id in the first column offers a safer way to determine the test instances.

Usually, if you evaluate a classifier for a longer experiment, you will do something like this (for csh):

```bash
 java -Xmx1024m weka.classifiers.trees.J48 -t data.arff -k -d J48-data.model >&! J48-data.out &
```
The -Xmx1024m parameter for maximum heap size enables the Java heap, where Java stores objects, to grow to a maximum size of 1024 Megabytes. There is no overhead involved, it just leaves more room for the heap to grow. The -*k* flag gives you some additional performance statistics. In case your model performs well, it makes sense to save it via *-d* - you can always delete it later! The implicit cross-validation gives a more reasonable estimate of the expected accuracy on unseen data than the training set accuracy. The output both of standard error and output should be redirected, so you get both errors and the normal output of your classifier. The last & starts the task in the background. Keep an eye on your task via *top* and if you notice the hard disk works hard all the time (for linux), this probably means your task needs too much memory and will not finish in time for the exam. ;-) In that case, switch to a faster classifier or use [filters](primer.md#weka-filters), e.g., for [Resample](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/supervised/instance/Resample.html) to reduce the size of your dataset or [StratifiedRemoveFolds](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/supervised/instance/StratifiedRemoveFolds.html) to create training and test sets - for most classifiers, training takes more time than testing.

So, now you have run a lot of experiments -- which classifier is best? Try
```bash
 cat *.out | grep -A 3 "Stratified" | grep "^Correctly"
```
...this should give you all cross-validated accuracies. If the cross-validated accuracy is roughly the same as the training set accuracy, this indicates that your classifiers is presumably not overfitting the training set.

Assume you have found the best classifier. To apply it on a new dataset, use something like
```bash
 java weka.classifiers.trees.J48 -l J48-data.model -T new-data.arff
```
You will have to use the same classifier to load the model, but you need not set any options. Just add the new test file via *-T*. If you want, *-p first-last* will output all test instances with classifications and confidence scores, followed by all attribute values, so you can look at each error separately.

The following more complex csh script creates datasets for learning curves, creating a 75% training set and 25% test set from a given dataset, then successively reducing the test set by factor 1.2 (83%), until it is also 25% in size. All this is repeated thirty times, with different random reorderings (-*S*) and the results are written to different directories. The Experimenter GUI in WEKA can be used to design and run similar experiments.
```bash
#!/bin/csh
foreach f ($*)
  set run=1
  while ( $run <= 30 )
    mkdir $run >&! /dev/null
    java weka.filters.supervised.instance.StratifiedRemoveFolds -N 4 -F 1 -S $run -c last -i ../$f -o $run/t_$f
    java weka.filters.supervised.instance.StratifiedRemoveFolds -N 4 -F 1 -S $run -V -c last -i ../$f -o $run/t0$f
    foreach nr (0 1 2 3 4 5)
      set nrp1=$nr
      @ nrp1++
      java weka.filters.supervised.instance.Resample -S 0 -Z 83 -c last -i $run/t$nr$f -o $run/t$nrp1$f
    end

    echo Run $run of $f done.
    @ run++
  end
end
```

If meta classifiers are used, i.e. classifiers whose options include classifier specifications - for example, [StackingC](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/meta/StackingC.html) or [ClassificationViaRegression](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/meta/ClassificationViaRegression.html), care must be taken not to mix the parameters. For example,
```bash
 java weka.classifiers.meta.ClassificationViaRegression -W weka.classifiers.functions.LinearRegression -S 1 \
   -t data/iris.arff -x 2
```
gives us an illegal options exception for *-S 1*. This parameter is meant for LinearRegression, not for ClassificationViaRegression, but WEKA does not know this by itself. One way to clarify this situation is to enclose the classifier specification, including all parameters, in "double" quotes, like this:

```bash
 java weka.classifiers.meta.ClassificationViaRegression -W "weka.classifiers.functions.LinearRegression -S 1" \
   -t data/iris.arff -x 2
```
However this does not always work, depending on how the option handling was implemented in the top-level classifier. While for Stacking this approach would work quite well, for ClassificationViaRegression it does not. We get the dubious error message that the class *weka.classifiers.functions.LinearRegression -S 1* cannot be found.
Fortunately, there is another approach: 
All parameters given after -- are processed by the first sub-classifier; another -- lets us specify parameters for the second sub-classifier and so on.
```bash
 java weka.classifiers.meta.ClassificationViaRegression -W weka.classifiers.functions.LinearRegression \
   -t data/iris.arff -x 2 -- -S 1
```
In some cases, both approaches have to be mixed, for example:

```bash
 java weka.classifiers.meta.Stacking -B "weka.classifiers.lazy.IBk -K 10" \
   -M "weka.classifiers.meta.ClassificationViaRegression -W weka.classifiers.functions.LinearRegression -- -S 1" \
   -t data/iris.arff -x 2
```
Notice that while ClassificationViaRegression honors the -- parameter, Stacking itself does not.
