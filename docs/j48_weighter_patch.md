

# Description
J48-Weighter patch: 
Modification of J48 for Weighted Data.

# Reference 
-none-

# Package
Patches to:

* `weka.classifiers.trees.j48`
* `weka.core`
* `weka.filters.unsupervised.attribute`

# Download
Patch for Weka 3.4.5: 
[j48-weighter.patch](files/j48-weighter.patch)

# Additional Information
This patch addresses two separate but related issues:

* The proposed filter "Weighter" allows one to specify a numeric attribute to be used as an instance weight.
* As mentioned on Wekalist, tests using weighted sample-survey data indicated possible problems in the J48 decision tree algorithm: 
[wekalist/2004-December/003135](https://list.waikato.ac.nz/pipermail/wekalist/2004-December/003135.html)

## The Weighter filter
Weighter is a general-purpose filter independent of J48 or other
classifiers, but to preserve the weight assignment it initially had to
be run under FilteredClassifier.  To make weights persistent via .arff
files, some changes were made in Instances and Instance, while retaining
compatibility with the existing [ARFF](formats_and_processing/arff.md) format.

Briefly, if Weighter is applied to an attribute, e.g. "fnlwgt" in the
"adult" dataset from the UCI repository, that attribute is removed and
its value is used as instance weight.  Upon Save, the weight is appended
to each instance under the attribute name "::weight::fnlwgt"; reading
the .arff file inverts the Save process, transparent to the user.

Repeated application of Weighter multiplies the weight and extends its
name.  The special case of invoking Weighter without an attribute
argument restores the unweighted dataset, with an appended attribute
named as above.

**Note:** the [XRFF](formats_and_processing/xrff.md) format, introduced in 3.5.4, stores instance and attribute weights, as opposed to the default [ARFF](formats_and_processing/arff.md) format.

## J48 with instance weights
The simple rescaling inserted in weka.classifiers.trees.j48.Stats is
intended to:

* use the correct sample size in the normal approximation to the binomial,
* make the scale of the .5 continuity correction consistent with the data,
* base the minimum-leaf-count option (-M) on unweighted counts.

These changes make pruning more effective with weighted data, and help
to reduce apparent overfitting.  This should be the case whether the
weights reflect missing value imputation (as is common in Weka), or
survey-sampling probabilities (e.g. "fnlwgt" in the UCI "adult"
sample).

The modification to j48.Stats would not have worked on its own.  In
particular, j48.Distribution had been written to maintain one set of
counts only.  To work on weighted data statistical algorithms often
require both weighted and unweighted counts.

A few other minor modifications were introduced to change the way "-M"
works.  One effect is that, for this purpose, instances with missing
x-values are no longer counted; they are considered missing.
