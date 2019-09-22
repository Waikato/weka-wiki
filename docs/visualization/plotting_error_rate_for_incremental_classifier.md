The KnowledgeFlow enables one to plot the error rate (= RMSE, root mean squared error) and the accuracy of an incremental classifier. An *incremental classifier* is a classifier that does not need to see the whole data at once, but can be trained instance by instance. All classifiers implementing the interface `[weka.classifiers.UpdateableClassifier](http://weka.sourceforge.net/doc.dev/weka/classifiers/updateableclassifier.html)` are incremental ones.

# Setup
The most basic setup for an incremental classifier is show below, using the classifier `NaiveBayesUpdateable`:

```text
 ArffLoader
   --instance--> NaiveBayesUpdateable
   --incrementalClassifier--> IncrementalClassifierEvaluator
   --chart--> StripChart
```
Here is a screenshot of the setup:


![Screenshot](../img/KnowledgeFlow-incremental_classifier-Setup.png)

You can also download [KnowledgeFlow-incremental_classifier.kfml](../files/KnowledgeFlow-incremental_classifier.kfml), an XML version of this setup.

# Displaying the chart
* select a dataset that you want to train the classifier with, via *Configure...* from the `ArffLoader's` context menu, e.g., the UCI dataset *anneal*.
* select *Show chart* from the `StripChart's` context menu (the chart will **only** be updated if visible!)
* select *Start loading* from the `ArffLoader's` context menu.
* bring the chart back to front and you should get a graph similar to this one (click to enlarge):


![Screenshot](../img/KnowledgeFlow-incremental_classifier-StripChart.png)

# Exporting the chart
As of 08/07/2008 (or Weka >3.5.7), the chart can be exported to several file formats using the developer version of Weka. The *magic* key for bringing up the export dialog is `<Ctrl+Alt+Shift><Left-Click>`.

Since the default black background is not very practical if one wants to embed the chart in a document, one can change the background color via the following property of the [Beans.props](../weka_gui_beans_beans.props.md) [properties file](../properties_file.md) (and set it to *white*):

```text
 weka.gui.beans.StripChart.backgroundColour
```
The text color of the legend can be modified via the following property (and set it to *black*):

```text
 weka.gui.beans.StripChart$LegendPanel.borderColour
```
**Note:** Due to the design of the StripChart (nested JPanels), the EPS export does not work properly. But one can always export it as PNG and then convert it under Linux via the `pngtopnm/pnmtops/ps2epsi` chain. See commandline help of those tools for more details.

# See also
* [Properties File](../properties_file.md)
* [weka/gui/beans/Beans.props](../weka_gui_beans_beans.props.md)

# Links
* [KnowledgeFlow-incremental_classifier.kfml](../files/KnowledgeFlow-incremental_classifier.kfml)
