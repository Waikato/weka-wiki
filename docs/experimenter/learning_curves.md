

The *Advanced mode* of the Experimenter can be used to generated learning curves for classifiers. These approaches can be setup in the *Simple mode* as well, but it is more cumbersome than in the advanced mode.

# Number of instances
For varying the number of instances a classifier is trained on, we use the `FilteredClassifier` classifier (package `weka.classifiers.meta`) in conjunction with the `RemovePercentage` filter (package `weka.filters.unsupervised.instance`) and J48 as base classifier (package `weka.classifiers.trees`):

* start the Experimenter (class `weka.gui.experiment.Experimenter`)
* select the configuration mode *Advanced* in the *Setup* panel
* choose as *Destination* either an ARFF file (= `InstancesResultListener`) or a [database](../databases.md) (= `DatabaseResultListener`) and configure the listener to your needs
* choose as *Result generator* the `CrossValidationResultProducer` (or leave the `RandomSplitResultProducer`)
* open the options dialog of the `CrossValidationResultProducer` by left-clicking on the edit field
* in case of regression datasets, choose the `RegressionSplitEvaluator` instead of the `ClassifierSplitEvaluator` (the latter is used for classification problems)
* open the options dialog for the *splitEvaluator* by left-clicking on the edit field
* *choose* the classifier that you want to analyze and setup it's parameters, in our case this is `FilteredClassifier` with `J48` as base classifier and `RemovePercentage` as filter
* close all dialogs again (accepting them with OK)
* set the *Generator properties* to *enabled*
* choose as property *percentage* and click on *Select*:

```text
 splitEvaluator -> classifier -> filter -> percentage
```
* now you can add all the percentages that you want to test, e.g. (NB: this is the percentage being *removed*!):

```text
 90, 80, 70, 60, 50, 40, 30, 20, 10
```
* add the datasets you want to generate the learning curve for
* save the experiment
* go to the *Run* panel and start the experiment
* after the experiment has finished, select the *Analyse* panel and perform your analysis on the results

# Classifier parameter
This example shows how to generate a learning curve that does not vary on the number of instances, but on a specific classifier parameter, e.g., the *confidenceFactor* (= commandline option `-C`) of `J48`. 

* start the Experimenter (class `weka.gui.experiment.Experimenter`)
* select the configuration mode *Advanced* in the *Setup* panel
* choose as *Destination* either an ARFF file (= `InstancesResultListener`) or a [database](../databases.md) (= `DatabaseResultListener`) and configure the listener to your needs
* choose as *Result generator* the `CrossValidationResultProducer` (or leave the `RandomSplitResultProducer`)
* open the options dialog of the `CrossValidationResultProducer` by left-clicking on the edit field
* in case of regression datasets, choose the `RegressionSplitEvaluator` instead of the `ClassifierSplitEvaluator` (the latter is used for classification problems)
* open the options dialog for the *splitEvaluator* by left-clicking on the edit field
* *choose* the classifier that you want to analyze and setup it's parameters, in our case this is `J48`
* close all dialogs again (accepting them with OK)
* set the *Generator properties* to *enabled*
* choose as property *percentage* and click on *Select*:

```text
 splitEvaluator -> classifier -> confidenceFactor
```
* now you can add all the factors that you want to test, e.g.:

```text
 0.10, 0.15, 0.20, 0.25, 0.30, 0.35, 0.40, 0.45, 0.50
```
* add the datasets you want to generate the learning curve for
* save the experiment
* go to the *Run* panel and start the experiment
* after the experiment has finished, select the *Analyse* panel and perform your analysis on the results

# See also
* [Databases](../databases.md)
