Basically, a classifier needs to be derived from `weka.classifiers.Classifier` and a filter from `weka.filters.Filter`, but this is only part of the story. The following articles cover the development of new schemes in greater detail:

* [Writing your own Classifier](../writing_classifier.md)
* [Writing your own Filter](../writing_filter.md)

If your scheme is outside the usual WEKA packages, you need to make WEKA aware of this package in order to be able to use it in the GUI as well. See [How do I add a new classifier, filter, kernel, etc?](how_do_i_add_a_new_classifier_filter_kernel_etc.md) for more information about this.

**Note:** This is also covered in chapter *Extending WEKA* of the WEKA manual in versions later than 3.6.1/3.7.0. Furthermore, this chapter also covers clusterers, attribute selection algorithms and associators.