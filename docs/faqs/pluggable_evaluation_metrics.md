WEKA 3.7.8 has a mechanism to allow new classification and regression evaluation metrics to be added as plugins. The new metrics will be output, along with WEKA's standard set of evaluation metrics, in the output generated on the command line, in the Explorer's Classify panel and by the Knowledge Flow's ClassifierPerformanceEvaluator component. Furthermore, new plugin metrics are also available for analysis in the Experimenter.

Previously, adding a new evaluation metric involved editing and recompiling the monolithic weka.classifiers.Evaluation class - a shudder-worthy undertaking at the best of times. With the new plugin mechanism it is easy to add a new metric and deploy it via the package management system. The "Additional configuration files" section of [How are packages structured for the package management system?](../packages/structure.md) details how to tell the PluginManager class about your new plugin evaluation metric.

# Classes and interfaces

The main base class for all new metrics is `weka.classifiers.evaluation.AbstractEvaluationMetric`. This class requires the following methods to be implemented by concrete sub classes:

* `boolean appliesToNominalClass()` - true if the stats computed by this metric apply to nominal class problems
* `boolean appliesToNumericClass()` - true if the stats computed by this metric apply to numeric class problems
* `String getMetricName()` - return the name of the metric
* `String getMetricDescription()` - return a short description of the metric
* `List<String> getStatisticNames()` - return a list of statistics that this metric computes (e.g. a "correct" metric might return both the number correctly classified and the percentage correct)
* `double getStatistic(String statName)` - get the computed value for the named statistic

To facilitate computing statistics, the main Evaluation object (who's class now lives in weka.classifiers.evaluation) will pass a reference to itself to all plugin metrics when it is first constructed. Therefore, a plugin metric has access to all the protected fields in the Evaluation class, and can use these when computing it's own statistic(s).

Beyond extending `AbstractEvaluationMetric`, a plugin metric will also need to implement one of the following interfaces:

## `weka.classifiers.evaluation.StandardEvaluationMetric`

Interface for a "standard" evaluation metric - i.e. one that would be part of the normal output in WEKA without having to turn on specific display options.

It defines the following methods

* `String toSummaryString()` - return a formatted string (suitable for displaying in the console or GUI output) that contains all the statistics that this metric computes
* `void updateStatsForClassifier(double[] predictedDistribution, Instance instance)` - updates the statistics about a classifiers performance for the current test instance. Gets called when the class is nominal. Implementers need only implement this method if it is not possible to compute their statistics from what is stored in the base Evaluation object.
* `void updateStatsForPredictor(double predictedValue, Instance instance)` -  updates the statistics about a predictors performance for the current test instance. Gets called when the class is numeric. Implementers need only implement this method if it is not possible to compute their statistics from what is stored in the base Evaluation object.

## `weka.classifiers.evaluation.InformationTheoreticEvaluationMetric`

Interface for information theoretic evaluation metrics to implement. Allows the command line interface to display these metrics or not based on user-supplied options.

It defines the following methods

* `String toSummaryString()` - return a formatted string (suitable for displaying in the console or GUI output) that contains all the statistics that this metric computes
* `void updateStatsForClassifier(double[] predictedDistribution, Instance instance)` - updates the statistics about a classifiers performance for the current test instance. Gets called when the class is nominal. Implementers need only implement this method if it is not possible to compute their statistics from what is stored in the base Evaluation object.
* `void updateStatsForPredictor(double predictedValue, Instance instance)` -  updates the statistics about a predictors performance for the current test instance. Gets called when the class is numeric. Implementers need only implement this method if it is not possible to compute their statistics from what is stored in the base Evaluation object.
* `void updateStatsForConditionalDensityEstimator(ConditionalDensityEstimator classifier, Instance classMissing, double classValue)` -  updates stats for conditional density estimator based on current test instance. Gets called when the class is numeric and the classifier is a ConditionalDensityEstimators. Implementers need only implement this method if it is not possible to compute their statistics from what is stored in the base Evaluation object.

## `weka.classifiers.evaluation.InformationRetrievalMetric`

 An interface for information retrieval evaluation metrics to implement. Allows the command line interface to display these metrics or not based on user-supplied options. These statistics will be displayed as new columns in the table of information retrieval statistics. As such, a toSummaryString() formatted representation is not required.

It defines the following methods

* `void updateStatsForClassifier(double[] predictedDistribution, Instance instance)` - updates the statistics about a classifiers performance for the current test instance. Gets called when the class is nominal. Implementers need only implement this method if it is not possible to compute their statistics from what is stored in the base Evaluation object.
* `double getStatistic(String name, int classIndex)` - get the value of the named statistic for the given class index. If the implementing class is extending AbstractEvaluationMetric then the implementation of getStatistic(String statName) should just call this method with a classIndex of 0.
* `double getClassWeightedAverageStatistic(String statName)` - get the weighted (by class) average for this statistic.

## `weka.classifiers.evaluation.IntervalBasedEvaluationMetric`

Primarily a marker interface for interval-based evaluation metrics to implement. Allows the command line interface to display these metrics or not based on user-supplied options.

It defines the following methods

* `String toSummaryString()` - return a formatted string (suitable for displaying in the console or GUI output) that contains all the statistics that this metric computes
* `void updateStatsForIntervalEstimator(IntervalEstimator classifier, Instance instance, double classValue)` - updates stats for interval estimator based on current test instance. Implementers need only implement this method if it is not possible to compute their statistics from what is stored in the base Evaluation object.
