You save a trained classifier with the `-d` option (*dumping*), e.g.:

```bash
 java weka.classifiers.trees.J48 -C 0.25 -M 2 -t /some/where/train.arff -d /other/place/j48.model
```
And you can load it with `-l` and use it on a test set, e.g.:

```bash
 java weka.classifiers.trees.J48 -l /other/place/j48.model -T /some/where/test.arff
```
Note, when loading a model you no longer need to supply specific parameters to the classifier.

## Explorer

A trained model can be saved like this, e.g., J48:

* train your model on the training data `/some/where/train.arff`
* right-click in the *Results list* on the item which model you want to save
* select *Save model* and save it to `/other/place/j48.model`

You can load the previously saved model with the following steps:

* load your test data `/some/where/test.arff` via the *Supplied test set* button
* right-click in the *Results list*, select *Load model* and choose `/other/place/j48.model`
* in the *More options* dialog, change the *Output predictions* to *CSV* or another format (and specify a file in the 
  options of the output format), if you want to store the predictions in a file rather than having to copy/paste them
* select *Re-evaluate model on current test set*

Based on [this](https://list.waikato.ac.nz/pipermail/wekalist/2006-August/007720.html) [Weka Mailing List](mailing_list.md) post.

## Making Predictions with your model without retraining

See the [Making predictions](making_predictions.md) article for detailed information.

## Source code

See [Serialization](serialization.md) for code examples.

# See also

* [Serialization](serialization.md)