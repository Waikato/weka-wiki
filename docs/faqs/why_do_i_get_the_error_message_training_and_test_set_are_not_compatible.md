One of WEKA's fundamental assumption is that the structure of the training and test sets are **exactly** the same. This does not only mean that you need the exact *same number* of attributes, but also the exact *same type*. In case of *nominal* attributes, you must ensure that the *number* of labels and the *order* of the labels are the same.

This may seem odd, as for [making predictions](../making_predictions.md) with a trained classifier, you wouldn't need to include any class attribute information. This is true from a human perspective, but for speed reasons, WEKA doesn't perform any checks regarding the structure of dataset (no mapping of attribute names from training space to test space, also no mapping of labels). Internally, a single row in a dataset is represented as an array of doubles. In case of *numeric* attributes, this doesn't pose a problem, but for other attribute types, like *nominal* ones, the doubles represent indexes in the list of available labels. A different order of the labels would result in different labels represented by the same index. Predictions cannot be trusted then.

Now, if you want to quickly check where the problem is, a visual diff program is very helpful. There is a plethora of [applications](http://en.wikipedia.org/wiki/comparison_of_file_comparison_tools) available. To name a few cross-platform open-source ones: 

* [kdiff3](http://kdiff3.sourceforge.net/)
* [kompare](http://www.caffeinated.me.uk/kompare/)
* [diffuse](http://diffuse.sourceforge.net/)

If you used a filter for processing training and test set, then have a look at FAQ [How do I generate compatible train and test sets that get processed with a filter?](how_do_i_generate_compatible_train_and_test_sets_that_get_processed_with_a_filter.md)