

# File
`weka/gui/experiment/Experimenter.props`

# Description
Used for customizing the initial Experimenter settings.

# Version
* \>= 3.4.6
* \>= 3.5.1

# Fields
* `Extension`
> the default extension in the file-dialog (and therefore format)
	* `.exp` - uses Java [Serialization](serialization.md)
	* `[.xml](xml#serialization of experiments.md)`
	* `[.koml](xml#serialization of experiments.md)`
* `Destination` - *simple*
> the default destination
	* ARFF file
	* CSV file
	* JDBC database
* `ExperimentType` - *simple*
> the experiment type
	* Cross-validation
	* Train/Test Percentage Split (data randomized)
	* Train/Test Percentage Split (order preserved)
* `UseClassification` - *simple*
> whether classification is the default (`true`) or regression (`false`)
* `Folds` - *simple*
> the default number of CV folds
* `TrainPercentage` - *simple*
> the default percentage for training (0 - 100)
* `Repetitions` - *simple*
> the default number of repetitions
* `DatasetsFirst`
> whether datasets are first iterated (`true`) or the algorithms (`false`)
* `InitialDatasetsDirectory`
> the initial datasets directory
> Note for Win32: 
the path backslashes have to written as "\\"
* `UseRelativePaths`
> whether to use relative paths (`true`) or absolute ones (`false`)
* `Tester`
> the default tester to use
	* Paired T-Tester (corrected)
	* Paired T-Tester
* `Row`
> the row selection
* `Column`
> the column selection
* `ComparisonField`
> the default comparison field (lower case!), cf. combobox
* `Significance`
> the default significance (0.0 - 1.0)
* `Sorting`
> the default sorting, left empty means no sorting at all
* `ShowStdDev`
> whether stddevs are displayed by default
* `ShowAverage`
> whether the Average is displayed by default (prints an additional list in the results)
* `MeanPrecision`
> the default precision for the mean
* `StdDevPrecision`
> the default precision for the stdev
* `OutputFormat`
> the classname of the ResultMatrix, responsible for the default output format (see `weka.experiment` package)
* `RemoveFilterClassnames`
> whether filter classnames are removed by default

**Note:** *simple* means that this option is only available in the *simple* version of the Experimenter, not the *advanced* one

# See also
* [Properties File](properties_file.md)
