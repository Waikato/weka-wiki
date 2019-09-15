AUC = the **A**rea **U**nder the ROC **C**urve. 

Weka uses the [Mann Whitney statistic](http://en.wikipedia.org/wiki/Mann-Whitney_U) to calculate the AUC via the [weka.classifiers.evaluation.ThresholdCurve](http://weka.sourceforge.net/doc.dev/weka/classifiers/evaluation/ThresholdCurve.html) class.

# Explorer
See [ROC curves](roc_curves.md).

# KnowledgeFlow
See [ROC curves](roc_curves.md).

# Commandline
Classifiers can output the AUC if the *-i* option is provided. The *-i* option provides detailed information per class. 

Running the J48 classifier on the iris UCI [[Datasets|dataset]] with the following commandline:

```
 java [CLASSPATH|-classpath <your-classpath>] weka.classifiers.trees.J48 -t /some/where/iris.arff -i
```

produces this output:

```
 == Detailed Accuracy By Class ==
 
 TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
   0.98      0          1         0.98      0.99       0.99     Iris-setosa
   0.94      0.03       0.94      0.94      0.94       0.952    Iris-versicolor
   0.96      0.03       0.941     0.96      0.95       0.961    Iris-virginica
```

# See also
* [ROC curves](roc_curves.md)
* [Mann Whitney statistic](http://en.wikipedia.org/wiki/Mann-Whitney_U) on WikiPedia

# Links
* [University of Nebraska Medical Center, Interpreting Diagnostic Tests](http://gim.unmc.edu/dxtests/roc3.htm)
* [weka.classifiers.evaluation.ThresholdCurve](http://weka.sourceforge.net/doc.dev/weka/classifiers/evaluation/ThresholdCurve.html)

