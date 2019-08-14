
# General
The [ExperimentDemo.java](files/ExperimentDemo.java) class demonstrates the use of the Experiment API (stable 3.6 or developer version):

* setting up an experiment
	* one classifier
	* one or more datasets
	* classification or regression
	* cross-validation or random split
* running the experiment
* evaluating the experiment and outputting the results

Classes of the Experiment API being used:

* `weka.experiment.Experiment` - the class for peforming experiments
* `weka.experiment.ClassifierSplitEvaluator` - for classification
* `weka.experiment.RegressionSplitEvaluator` - for regression
* `weka.experiment.CrossValidationResultProducer` - for cross-validation
* `weka.experiment.RandomSplitResultProducer` - for random splits
* `weka.experiment.InstancesResultListener` - for storing the results of the experiment, used as input for the TTester algorithm
* `weka.experiment.PairedCorrectedTTester` - for generating the statistics
> see [Claude Nadeau, Yoshua Bengio (2001). Inference for the Generalization Error. Machine Learning.](http://www.iro.umontreal.ca/~lisa/bib/pub_subject/comparative/pointeurs/nadeau_MLJ1597.pdf)
* `weka.experiment.ResultMatrixPlainText` - for storing the statistics

# Examples
Usage: 

```bash
 java ExperimentDemo
   -classifier <classifier incl. parameters>
   -exptype <classification|regression>
   -splittype <crossvalidation|randomsplit>
   -runs <# of runs>
   -folds <folds for CV>
   -percentage <percentage for randomsplit>
   -result <ARFF file for storing the results>
   -t <dataset> (can be supplied multiple times)
```

## Classification 
An example run with J48 and two UCI datasets:

```bash
 java ExperimentDemo
   -classifier weka.classifiers.trees.J48
   -exptype classification
   -splittype crossvalidation
   -runs 10
   -folds 10
   -result /some/where/results.arff
   -t vote.arff
   -t iris.arff
```
And the output:

```text
  Setting up...
  Initializing...
  Running...
  Finishing...
  Evaluating...
  
  Result:

  (1) vote
      Perc. correct: 
96.57135311000002
      StdDev: 
2.560851001842444
  (2) iris
      Perc. correct: 
94.73333325999994
      StdDev: 
5.300826810632913
```

## Regression

Another example with M5P and two numeric UCI datasets:

```bash
 java ExperimentDemo
   -classifier weka.classifiers.trees.M5P
   -exptype regression
   -splittype randomsplit
   -runs 10
   -percentage 66
   -result /some/where/results.arff
   -t bolts.arff
   -t bodyfat.arff
```
And the associated output:

```text
  Setting up...
  Initializing...
  Running...
  Finishing...
  Evaluating...
  
  Result:

  (1) bolts
      Perc. correct: 
0.9701825
      StdDev: 
0.017970627641614084
  (2) bodyfat.names
      Perc. correct: 
0.9795883
      StdDev: 
0.011646527074622525
```

# See also
* [Use Weka in your Java code](use_weka_in_your_java_code.md) - for general use of the Weka API

# Downloads
* [ExperimentDemo.java](files/ExperimentDemo.java)
