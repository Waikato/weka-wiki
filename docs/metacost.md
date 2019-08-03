This metaclassifier makes its base classifier cost-sensitive using the method specified in:


*Pedro Domingos: MetaCost: A general method for making classifiers cost-sensitive. In: 
Fifth International Conference on Knowledge Discovery and Data Mining, 155-164, 1999.*

This classifier should produce similar results to one created by passing the base learner to Bagging, which is in turn passed to a CostSensitiveClassifier operating on minimum expected cost. The difference is that MetaCost produces a single cost-sensitive classifier of the base learner, giving the benefits of fast classification and interpretable output (if the base learner itself is interpretable). This implementation uses all bagging iterations when reclassifying training data (the MetaCost paper reports a marginal improvement when only those iterations containing each training instance are used in reclassifying that instance).

# Examples
The following cost matrix is used for a 3-class problem:

```text
 -3  1  1
  1 -6  1
  0  0  0
```
MetaCost will compute the costs (*Costs*) based on the class distribution the bagged base learner returns (*Class probs*) and select the class with the lowest cost (*Chosen class*):

```text
+---------------+-----------------+--------------+
| Class probs   | Costs           | Chosen class |
+---------------+-----------------+--------------+
| 1.0, 0.0, 0.0 | -3.0,  1.0, 1.0 |      1       |
| 0.0, 1.0, 0.0 |  1.0, -6.0, 1.0 |      2       |
| 0.0, 0.0, 1.0 |  0.0,  0.0, 0.0 |      1 *     |
| 0.7, 0.1, 0.2 | -2.0,  0.1, 0.8 |      1       |
| 0.2, 0.7, 0.1 |  0.1, -4.0. 0.9 |      2       |
| 0.1, 0.2, 0.7 | -0.1, -1.1, 0.3 |      2       |
+---------------+-----------------+--------------+
```
``*`` in case of a tie, the first one will be picked.

# See also
* [CostSensitiveClassifier](cost_sensitive_classifier.md)
* [CostMatrix](cost_matrix.md)

# Links
* [Publication on CiteSeer](http://citeseer.ist.psu.edu/domingos99metacost.html)
