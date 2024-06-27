

# Introduction
As of Weka version >3.5.8 (only developer version, not stable-3.6 branch) one can easily add graph visualization plugins in the Explorer (Classify panel). This makes it easy to implement custom visualizations, if the ones Weka offers are not sufficient.

**Note:** This is also covered in chapter *Extending WEKA* of the WEKA manual in versions later than 3.7.0.

# Requirements
* custom visualization class must implement the following **interface**
> `weka.gui.visualize.plugins.ErrorVisualizePlugin`
* the class must **either** reside in the following **package** (visualization classes are automatically discovered during run-time)
> `weka.gui.visualize.plugins`
* or the class' package must be listed in the `weka.gui.visualize.plugins.ErrorVisualizePlugin` key of the [weka/gui/GenericPropertiesCreator.props](../weka_gui_generic_properties_creator.props.md) file.

# Implementation
The visualization interface contains the following four methods

* **getMinVersion**
> This method returns the *minimal* version (inclusive) of Weka that is necessary to execute the plugin, e.g., `3.5.0`.
> * **getMaxVersion**
> This method returns the *maximal* version (exclusive) of Weka that is necessary to execute the plugin, e.g., `3.6.0`.
* **getDesignVersion**
> Returns the actual version of Weka this plugin was designed for, e.g., `3.5.1`
* **getVisualizeMenuItem**
> The `JMenuItem` that is returned via this method will be added to the plugins menu in the popup in the Explorer. The ActionListener for clicking the menu item will most likely open a new frame containing the visualized data.

# Examples
The following screenshots were generated using `weka.classifiers.functions.LinearRegression` with default parameters on the UCI [dataset](../datasets.md) *bolts*, using a percentage split of 66% for the training set and the remainder for testing.

## Using Weka panels
The [ClassifierErrorsWeka.java](../files/ClassifierErrorsWeka.java) example simply displays the classifier errors as the *Visualize classifier errors* menu item already available in Weka. It is just to demonstrate how to use existing Weka classes. 

![Screenshot](../img/ClassifierErrorsWeka.png)

## Using JMathtools' Boxplot
The [ClassifierErrorsMathtools.java](../files/ClassifierErrorsMathtools.java). The relative error per prediction is displayed as vertical line.

![Screenshot](../img/ClassifierErrorsMathtools.png)

**Note:** This display is only available for numeric classes.

# Downloads
* [ClassifierErrorsWeka.java](../files/ClassifierErrorsWeka.java)
* [ClassifierErrorsMathtools.java](../files/ClassifierErrorsMathtools.java)

# See also
* [Use Weka in your Java code](../use_weka_in_your_java_code.md) - general overview of the basic Weka API
* [Explorer visualization plugins](explorer_visualization_plugins.md)

# Links
* [JMathTools homepage](http://jmathtools.sourceforge.net/)
