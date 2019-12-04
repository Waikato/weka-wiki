<img style="float: right; margin: 10px; " width="250" src="../img/Breast-cancer_roc-curve.jpg">

# General
Weka just varies the threshold on the class probability estimates in each case. What does that mean? In case of a classifier that does not return proper class probabilities (like SMO with the -M option, or IB1), you will end up with only two points in the curve. Using a classifier that returns proper distributions, like BayesNet, J48 or SMO with -M option for building logistic models, you will get nice curves.

The class used for calculating the ROC and also the [AUC](auc.md) (= area under the curve) is [weka.classifiers.evaluation.ThresholdCurve](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/evaluation/ThresholdCurve.html).

# Commandline
You can output the data for the ROC curves with the following options:

```
 -threshold-file <file>
        The file to save the threshold data to.
        The format is determined by the extensions, e.g., '.arff' for ARFF
        format or '.csv' for CSV.
 -threshold-label <label>
        The class label to determine the threshold data for
        (default is the first label)
```

Here's an example for using J48 on the UCI dataset *anneal*, generating the ROC curve file for label *U* from a 10-fold cross-validation:

```bash
 java weka.classifiers.trees.J48 -t /some/where/anneal.arff \
      -threshold-file anneal_roc_U.arff -threshold-label U
```

# Explorer
## Generating
The Weka Explorer enables you to plot the ROC (*Receiver operating characteristic*) curve for a certain class label of dataset:

* run a classifier on a dataset
* right-click in the result list on the result you want to display the curve for
* select **Visualize threshold curve** and choose the class label you want the plot for

**Note:** the [AUC](auc.md) for this plot is also displayed, just above the actual plot.

## Saving 
You can save the ROC curve in two ways:

* as an [ARFF](formats_and_processing/arff.md) file, containing the data points (can be displayed again)
* as an **image** (using *Alt+Shift+Left click* to bring up a save dialog)

## Loading
A previously saved ROC data file can be displayed in two ways:

* without the [AUC](auc.md) - with the following command

    ```
    java [CLASSPATH|-classpath <your-classpath>] weka.gui.visualize.VisualizePanel <file>
    ```

* with the [AUC](auc.md) - needs [this](visualization/visualizing_roc_curve.md) source code

# KnowledgeFlow
See [Plotting multiple ROC curves](plotting_multiple_roc_curves.md).

# See also
* [Plotting multiple ROC curves](plotting_multiple_roc_curves.md)
* [Generating ROC curve](generating_roc_curve.md)

# Links
* [WikiPedia article on ROC curve](http://en.wikipedia.org/wiki/ROC_Curve)
* [weka.classifiers.evaluation.ThresholdCurve](https://weka.sourceforge.io/doc.stable-3-8/weka/classifiers/evaluation/ThresholdCurve.html)

