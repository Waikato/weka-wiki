A meta classifier that makes its base classifier cost-sensitive. Two
methods can be used to introduce cost-sensitivity: reweighting
training instances according to the total cost assigned to each class;
or predicting the class with minimum expected misclassification cost
(rather than the most likely class). Performance can often be improved
by using a bagged classifier to improve the probability estimates of
the base classifier.

Since the classifier, in default mode (i.e., when using the
reweighting method), *normalizes* the cost matrix before applying it,
it can be hard coming up with a cost matrix, e.g., one that balances
out imbalanced data. Here is an example:

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

The application of a [cost matrix](cost_matrix.md) using the second,
minimum-expected cost approach, which is also used by
[MetaCost](metacost.md), is more intuitive.

# See also

* [MetaCost](metacost.md)
* [CostMatrix](cost_matrix.md)