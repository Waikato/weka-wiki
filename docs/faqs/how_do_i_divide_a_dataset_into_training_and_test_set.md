You can use the `RemovePercentage` filter (package `weka.filters.unsupervised.instance`).

In the Explorer just do the following:

* training set:
	* Load the full dataset
	* select the `RemovePercentage` filter in the preprocess panel
	* set the correct percentage for the split
	* apply the filter
	* save the generated data as a new file
* test set:
	* Load the full dataset (or just use undo to revert the changes to the dataset)
	* select the `RemovePercentage` filter if not yet selected
	* set the `invertSelection` property to true
	* apply the filter
	* save the generated data as new file