

# Command line
The following sections show how to obtain predictions/classifications without writing your own Java code via the command line.

## Classifiers
After a [model has been saved](saving_and_loading_models.md), one can make predictions for a test set, whether that set contains valid class values or not. The output will contain both the actual and predicted class. (Note that if the test class contains simply '?' for the class label for each instance, the "actual" class label for each instance will not contain useful information, but the predicted class label will.) The `-T <test_set>` command-line switch specifies the dataset of instances whose classes are to be predicted, while the `-p <attribute_range>` switch allows the user to write out a range of attributes (examples: "1-2" for the first and second attributes, or "0" for no attributes). Sample command line:


```bash
 java weka.classifiers.trees.J48 -T unclassified.arff -l j48.model -p 0
```

The format of the output is as follows:


```text
<test_instance_index> <actual_class_index>:<actual_class_val> <pred_class_index>:<pred_class_val> [+| ] <prob_of_pred_class_val>
```

where "+" occurs only for those items that were mispredicted. Note that if the actual class label is always "?" (i.e., the dataset does not include known class labels), the error column will always be empty.

Sample output:

```text
 inst#     actual  predicted error prediction
     1        1:?        1:0       0.757 
     2        1:?        1:0       0.824 
     3        1:?        1:0       0.807 
     4        1:?        1:0       0.807 
     5        1:?        1:0       0.79 
     6        1:?        2:1       0.661 
   ...
```

In this case, taken directly from a test dataset where all class attributes were marked by "?", the "actual" column, which can be ignored, simply states that each class belongs to an unknown class. The "predicted" column shows that instances 1 through 5 are predicted to be of class 1, whose value is 0, and instance 6 is predicted to be of class 2, whose value is 1. The error field is empty; if predictions were being performed on a labeled test set, each instance where the prediction failed to match the label would contain a "+". The probability that instance 1 actually belongs to class 0 is estimated at 0.757.

**Notes:**

* Since Weka 3.5.4 you can also output the complete class distribution, not just the prediction, by using the parameter `-distribution` in conjunction with the -p option. In this case, "*" is placed beside the probability in the distribution that corresponds to the predicted class value.
* If you have an ID attribute in your dataset as first attribute (you can always add one with the [AddID](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/unsupervised/attribute/AddID.html) filter), you could output it with `-p 1` instead of using `-p 0`. This works only for explicit train/test sets, but you can use the Explorer for cross-validation.

## Filters
The `[AddClassification](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/supervised/attribute/addclassification.html)` filter (package `weka.filters.supervised.attribute`) can either train a classifier on the input data and transform this or load a serialized model to transform the input data (even though the filter was introduced in 3.5.4, due to a bug in the commandline option handling, it is recommended to download a version >3.5.5 or a snapshot from the Weka homepage).
This filter can add the classification, class distribution and the error per row as extra attributes to the dataset.

* training the classifier, e.g., J48, on the input data and replacing the class values with the ones of the trained classifier:

```bash
   java \
     weka.filters.supervised.attribute.AddClassification \
     -W "weka.classifiers.trees.J48" \
     -classification \
     -remove-old-class \
     -i train.arff \
     -o train_classified.arff \
     -c last
```
* using a serialized model, e.g., a J48 model, to replace the class values with the ones predicted by the serialized model:

```bash
   java \
     weka.filters.supervised.attribute.AddClassification \
     -serialized /some/where/j48.model \
     -classification \
     -remove-old-class \
     -i train.arff \
     -o train_classified.arff \
     -c last
```

# GUI
The Weka GUI allows you as well to output predictions based on a previously saved model.

## Explorer
See the [Explorer](saving_and_loading_models#explorer.md) section of the [Saving and loading models](saving_and_loading_models.md) article to setup the Explorer. Additionally, you need to check the *Output predictions* options in the *More options* dialog. Righ-clicking on the respective results history item and selecting *Re-evaluate model on current test set* will output then the predictions as well (the statistics will be useless due to missing class values in the test set, so just ignore them). The output is similar to the one produced by the commandline.

Example output for the *anneal* UCI dataset:

```text
 == Predictions on test set ==
 inst#,    actual, predicted, error, probability distribution
     1          ?        3:3      +   0      0     *1      0      0      0    
     2          ?        3:3      +   0      0     *1      0      0      0    
     3          ?        3:3      +   0      0     *1      0      0      0    
    ...
    17          ?        6:U      +   0      0      0      0      0     *1    
    18          ?        6:U      +   0      0      0      0      0     *1    
    19          ?        3:3      +   0      0     *1      0      0      0    
    20          ?        3:3      +   0      0     *1      0      0      0    
    ...
```
**Note:** The developer version (>3.5.6 or [snapshot](snapshots.md)) can also output additional attributes like the commandline with the `-p` option. In the *More options...* dialog you can specify those attribute indices with *Output additional attributes*, e.g., *first* or *1-7*. In contrast to the commandline, this output also works for cross-validation.

## KnowledgeFlow
## Using the PredictionAppender
With the *PredictionAppender* (from the *Evaluation* toolbar) you cannot use an already saved model, but you can train a classifier on a dataset and output an [ARFF](formats_and_processing/arff.md) file with the predictions appended as additional attribute. Here's an example setup:

```text
               /---dataSet--> TrainingSetMaker ---trainingSet--\
 ArffLoader --<                                                 >--> J48...
               \---dataSet--> TestSetMaker -------testSet------/
 
 ...J48 --batchClassifier--> PredictionAppender --testSet--> ArffSaver
```

## Using the AddClassification filter
The AddClassification filter can be used in the KnowledgeFlow as well, either for training a model, or for using a serialized model to perform the predictions. An example setup could look like this:

```text
 ArffLoader --dataSet--> ClassAssigner --dataSet--> AddClassification --dataSet--> ArffSaver
```

# Java
If you want to perform the classification within your own code, see the [classifying instances](use_weka_in_your_java_code.md#classifying-instances) section of [this article](use_weka_in_your_java_code.md), explaining the Weka API in general.

# See also
* [Saving and loading models](saving_and_loading_models.md)
* [Use Weka in your Java code](use_weka_in_your_java_code.md) - general information about using the Weka API
* [Using ID attributes](troubleshooting.md#instance-id)

# Version
The developer version shortly before the release of 3.5.6 was used as basis for this article.
