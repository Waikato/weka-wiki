

# Introduction
As of Weka version >3.5.8 (only developer version, not stable-3.6 branch) one can easily add graph visualization plugins in the Explorer (Classify panel). This makes it easy to implement custom visualizations, if the ones Weka offers are not sufficient. *graph* is referring to graphs generated, for instance, by the `weka.classifiers.bayes.BayesNet` classifier.

**Note:** This is also covered in chapter *Extending WEKA* of the WEKA manual in versions later than 3.7.0 or [snapshots](snapshots.md) of the developer version later than 10/01/2010.

# Requirements
* custom visualization class must implement the following **interface**
> `weka.gui.visualize.plugins.GraphVisualizePlugin`
* the class must **either** reside in the following **package** (visualization classes are automatically discovered during run-time)
> `weka.gui.visualize.plugins`
* or the class' package must be listed in the `weka.gui.visualize.plugins.GraphVisualizePlugin` key of the `[weka/gui/GenericPropertiesCreator.props](weka_gui_genericpropertiescreator.props.md)` file.

# Implementation
The visualization interface contains the following four methods

* **getMinVersion**
> This method returns the *minimal* version (inclusive) of Weka that is necessary to execute the plugin, e.g., `3.5.0`.
* **getMaxVersion**
> This method returns the *maximal* version (exclusive) of Weka that is necessary to execute the plugin, e.g., `3.6.0`.
* **getDesignVersion**
> Returns the actual version of Weka this plugin was designed for, e.g., `3.5.1`
* **getVisualizeMenuItem**
> The `JMenuItem` that is returned via this method will be added to the plugins menu in the popup in the Explorer. The ActionListener for clicking the menu item will most likely open a new frame containing the visualized data.

# Examples
## Prefuse visualization toolkit
The [PrefuseGraph.java](files/PrefuseGraph.java). It is based on the `prefuse.demos.GraphView` demo class.

The following screenshot was generated using *BayesNet* on the UCI dataset *anneal* with the following parametrization:

> `weka.classifiers.bayes.BayesNet -D -Q weka.classifiers.bayes.net.search.local.K2 -- -P 3 -S BAYES -E weka.classifiers.bayes.net.estimate.SimpleEstimator -- -A 0.5`

![Screenshot](img/PrefuseGraph.png)

# Downloads
* [PrefuseGraph.java](files/PrefuseGraph.java)

# See also
* [Use Weka in your Java code](use_weka_in_your_java_code.md) - general overview of the basic Weka API
* [Explorer visualization plugins](explorer_visualization_plugins.md)
* [Explorer tree visualization plugins](explorer_tree_visualization_plugins.md)

# Links
* [Prefuse homepage](http://prefuse.org/)
* [PAP - prefuse assistance pool](http://goosebumps4all.net/34all/bb/forumdisplay.php?fid=18)
