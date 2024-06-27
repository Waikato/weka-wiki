# KnowledgeFlow
## Description 
Comparing different classifiers on one dataset can also be done via [ROC curves](roc_curves.md), not just via Accuracy, Correlation coefficient etc. In the **Explorer** it is **not** possible to do that for several classifiers, this is only possible in the **KnowledgeFlow**.

This is the basic setup (based on a Wekalist post):

```
 ArffLoader 
 ---dataSet---> ClassAssigner 
 ---dataSet---> ClassValuePicker              (the class label you want the plot for)
 ---dataSet---> CrossValidationFoldMaker 
 ---trainingSet/testSet (i.e. BOTH connections)---> Classifier of your choice 
 ---batchClassifier---> ClassifierPerformanceEvaluator 
 ---thresholdData---> ModelPerformanceChart
```

![Screenshot](img/Knowledgeflow_breast-cancer_roc-curve_setup.jpg)

![Screenshot](img/Knowledgeflow_breast-cancer_roc-curve.jpg)

This setup can be easily extended to host several classifiers, which illustrates the [Plotting_multiple_roc.kfml](files/Plotting_multiple_roc.kfml) example, containing `J48` and `RandomForest` as classifiers.

# Java
## Description
The [VisualizeMultipleROC.java](files/VisualizeMultipleROC.java) class lets you display several ROC curves in a single plot. The data it is using for display is from previously saved ROC curves. This example class is just a modified version of the [VisualizeROC.java](files/VisualizeROC.java) class, which displays only a single ROC curve (see [Visualizing ROC curve](visualization/visualizing_roc_curve.md) article).

# See also
* [Wikipedia article on ROC curve](http://en.wikipedia.org/wiki/ROC_Curve)
* [Visualizing ROC curve](visualization/visualizing_roc_curve.md)
* [ROC curves](roc_curves.md)

# Downloads
* [Plotting_multiple_roc.kfml](files/Plotting_multiple_roc.kfml) - Example KnowledgeFlow layout file
* [VisualizeMultipleROC.java](files/VisualizeMultipleROC.java) ([stable](https://git.cms.waikato.ac.nz/weka/weka/-/tree/stable-3-8/wekaexamples/src/main/java/wekaexamples/gui/visualize/VisualizeMultipleROC.java), [developer](https://git.cms.waikato.ac.nz/weka/weka/-/tree/main/trunk/wekaexamples/src/main/java/wekaexamples/gui/visualize/VisualizeMultipleROC.java))

