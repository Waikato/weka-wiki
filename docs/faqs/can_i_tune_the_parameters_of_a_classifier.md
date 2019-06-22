Yes, you can do that with one of the following meta-classifiers:

* `weka.classifiers.meta.CVParameterSelection`
* `weka.classifiers.meta.GridSearch` (only developer version)
* `weka.classifiers.meta.AutoWEKAClassifier` (via external package)
* `weka.classifiers.meta.MultiSearch` (via [external package](https://github.com/fracpete/multisearch-weka-package))
 
See the Javadoc of the respective classifier or the [Optimizing parameters](../optimizing_parameters.md) article for more information.