

# File
weka/gui/explorer/Explorer.props

# Description
This props file determines what schemes and options are initially set in the Explorer.

# Version
* > 3.5.3

# Fields

## Preprocess panel

* InitGenericObjectEditorFilter
>> if set to true the Capabilities filters in the GOE will be initialized based on the full dataset that has been loaded into the Explorer otherwise only the header
* Tabs
>> Lists all the tabs that should be displayed in the Explorer. Apart from the Preprocess panel itself, all other panels are basically plugins.
>> See the [Adding tabs in the Explorer](adding_tabs_in_the_explorer.md) article for more details on adding custom panels.
* InitialDirectory (> 3.6.0, developer version)
>> Defines the initial directory for opening datasets in the Preprocess panel.
>> The following placeholders are recognized (work across platforms):

	* %t - the temp directory
	* %h - the user's home directory
	* %c - the current directory **(the default setting)**
	* %% - gets replaced by a single percentage sign
	
* enableUndo (> 3.6.5, > 3.7.4)
>> Enable/disable the creation of undo files (default is enabled)
* undoDirectory (> 3.6.5, > 3.7.4)
>> Specify the directory to use for saving undo files
>> The following placeholders are recognized (work across platforms):
	* %t - the temp directory

* Filter
>> the filter to use, none if left empty
## Classify panel
* Classifier
>> the classifier to use
* ClassifierTestMode
>> the default test mode in the classify tab
	* 1 - cross-validation (default)
	* 2 - percentage split
	* 3 - use training set
	* 4 - supplied test set
* ClassifierCrossvalidationFolds
>> the default number of folds for CV
* ClassifierCostSensitiveEval
>> whether the evaluation of the classifier is done cost-sensitively
>> a cost matrix still has to be provided!
* ClassifierOutputConfusionMatrix
>> whether the confusion matrix is output
* ClassifierOutputEntropyEvalMeasures
>> whether the entropy based evaluation measures of the classifier model are output
* ClassifierOutputModel
>> whether the classifier model is output
* ClassifierOutputPerClassStats
>> whether additional per-class stats of the classifier model are output
* ClassifierOutputPredictions
>> whether the predictions of the classifier output as well
* 	ClassifierPercentageSplit
>> the default percentage split in %
* ClassifierPreserveOrder
>> whether the order is preserved in case of percentage split
* ClassifierRandomSeed
>> the default random seed
* ClassifierStorePredictionsForVis
>> whether the predictions of the classifier are stored for visulization purposes
* ClassifierOutputSourceCode (> 3.5.5)
>> whether to output Java source code for classifiers that implement the weka.classifiers.Sourcable interface
* ClassifierSourceCodeClass (> 3.5.5)
>> the default classname of the generated Java source code
* ClassifierErrorsPlotInstances (> 3.7.0)
>> the default classname for the class generating the plot instances of the classifier errors
* ClassifierErrorsMinimumPlotSizeNumeric (> 3.7.0)
>> the minimum size for the crosses that display the classifier errors for numeric class attributes
* ClassifierErrorsMaximumPlotSizeNumeric (> 3.7.0)
>> the maximum size for the crosses that display the classifier errors for numeric class attributes


## Cluster panel
* Clusterer
>> the clusterer to use
* ClustererTestMode
>> the default test mode
	* 2 - percentage split
	* 3 - use training set (default)
	* 4 - supplied test set
	* 5 - classes to clusters evaluation
* ClustererStoreClustersForVis
>> whether the clusters are stored for visualization purposes
* **Associations panel**
* Associator
>> the default associator
* **Attribute selection panel**
* ASEvaluation
>> the default attribute evaluator
* ASSearch
>> the default attribute selection search scheme
* ASTestMode
>> the default test mode
	* 0 - use full training set (default)
	* 1 - cross-validation
* ASCrossvalidationFolds
>> the default number of folds for CV
* ASRandomSeed
>> the default random seed

# See also
* [Properties File](properties_file.md)
