A meta classifier that makes its base classifier cost-sensitive. Two methods can be used to introduce cost-sensitivity: 
reweighting training instances according to the total cost assigned to each class; or predicting the class with minimum expected misclassification cost (rather than the most likely class). Performance can often be improved by using a Bagged classifier to improve the probability estimates of the base classifier.

Since the classifier *normalizes* the cost matrix before applying it, it [makes it hard](https://list.waikato.ac.nz/pipermail/wekalist/2006-November/008569.html) coming up with a cost matrix, e.g., to balance out imbalanced data. Here's an example:

* input cost matrix
```
 -3  1  1
  1 -6  1
  0  0  0
```
* normalized cost matrix
```
 0 7 1
 4 0 1
 3 6 0
```
But still, according to [Weka users](https://list.waikato.ac.nz/pipermail/wekalist/2006-November/008589.html), one can set *the cost as to equalize the class distributions (i.e.*
*achieving a 1:1 class distribution afterward)*. But this could be limited to 2-class problems.

The application of the [cost matrix](cost_matrix.md) in [MetaCost](metacost.md) is more intuitive.

# See also

* [MetaCost](metacost.md)
* [CostMatrix](cost_matrix.md)