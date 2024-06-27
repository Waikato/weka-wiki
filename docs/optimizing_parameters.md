
Since finding the optimal parameters for a classifier can be a rather tedious process, Weka offers some ways of automating this process a bit. The following meta-classifiers allow you to optimize some parameters of your base classifier:

* `weka.classifiers.meta.CVParameterSelection`
* `weka.classifiers.meta.GridSearch` (only developer version)
* `weka.classifiers.meta.MultiSearch` ([external package](https://github.com/fracpete/multisearch-weka-package) for 3.7.11+)
* Auto-WEKA ([external package](https://www.cs.ubc.ca/labs/beta/Projects/autoweka/) package for 3.7.13+)

After finding the best possible setup, the meta-classifiers then train an instance of the base classifier with these parameters and use it for subsequent predictions.

# CVParameterSelection 
This meta-classifier can optimize over an arbitrary number of parameters, with only one drawback (apart from the obvious explosion of possible parameter combinations): 
one cannot optimize on nested options, only direct options of the base classifier. What does that mean? It means, that you can optimize the *C* parameter of `weka.classifiers.functions.SMO`, but not the *C* of an `weka.classifiers.functions.SMO` within a `weka.classifiers.meta.FilteredClassifier`.

Here are a few examples:

* **J48 and it's confidence interval ("-C")**
	* load your dataset in the Explorer
	* choose `weka.classifiers.meta.CVParameterSelection` as classifier
	* select `weka.classifiers.trees.J48` as base classifier within `CVParameterSelection`
	* open the ArrayEditor for *CVParameters* and enter the following string (and click on *Add*): `C 0.1 0.5 5` - This will test the confidence parameter from 0.1 to 0.5 with step size 0.1 (= 5 steps)

	* close dialogs and start the classifier
	* you will get output similar to this one, with the best parameters found in bold:

```
 Cross-validated Parameter selection.
 Classifier: 
weka.classifiers.trees.J48
 Cross-validation Parameter: 
'-C' ranged from 0.1 to 0.5 with 5.0 steps
 Classifier Options: 
**-C 0.1** -M 2
```

* **SMO and it's complexity parameter ("-C")**
	* load your dataset in the Explorer
	* choose `weka.classifiers.meta.CVParameterSelection` as classifier
	* select `weka.classifiers.functions.SMO` as base classifier within `CVParameterSelection` and modify its setup if necessary, e.g., RBF kernel
	* open the ArrayEditor for *CVParameters* and enter the following string (and click on *Add*):

> `C 2 8 4`
> This will test the complexity parameters 2, 4, 6 and 8 (= 4 steps)
	* close dialogs and start the classifier
	* you will get output similar to this one, with the best parameters found in bold:

```
 Cross-validated Parameter selection.
 Classifier: 
weka.classifiers.functions.SMO
 Cross-validation Parameter: 
'-C' ranged from 2.0 to 8.0 with 4.0 steps
 Classifier Options: 
**-C 8** -L 0.0010 -P 1.0E-12 -N 0 -V -1 -W 1 -K "weka.classifiers.functions.supportVector.RBFKernel -C 250007 -G 0.01"
```
* **[LibSVM](lib_svm.md) and the gamma parameter of the RBF kernel ("-G")**
	* load your dataset in the Explorer
	* choose `weka.classifiers.meta.CVParameterSelection` as classifier
	* select `[weka.classifiers.functions.LibSVM](lib_svm.md)` as base classifier within `CVParameterSelection` and modify its setup if necessary, e.g., RBF kernel
	* open the ArrayEditor for *CVParameters* and enter the following string (and click on *Add*):

> `G 0.01 0.1 10`
> This will iterate over the gamma parameter, using values from 0.01 to 0.1 (= 10 steps)
	* close dialogs and start the classifier
	* you will get output similar to this one, with the best parameters found in bold:

```
 Cross-validated Parameter selection.
 Classifier: 
weka.classifiers.functions.LibSVM
 Cross-validation Parameter: 
'-G' ranged from 0.01 to 0.1 with 10.0 steps
 Classifier Options: 
**-G 0.09** -S 0 -K 2 -D 3 -R 0.0 -N 0.5 -M 40.0 -C 1.0 -E 0.0010 -P 0.1
```

# GridSearch 
`weka.classifiers.meta.GridSearch` is a meta-classifier for exploring 2 parameters, hence the *grid* in the name. If one turns the log on, the classifier will create output suitable for [gnuplot](http://www.gnuplot.info/), i.e., sections of the log will contain script and data sections. Instead of just using a classifier, one can specify a base classifier **and** a filter, which both of them can be optimized (one parameter each). In contrast to `CVParameterSelection`, `GridSearch` is not limited to first-level parameters of the base classifier, since it's using [Java Beans](http://java.sun.com/docs/books/tutorial/javabeans/) [Introspection](http://java.sun.com/docs/books/tutorial/javabeans/introspection/) and one can specify *paths* to the properties one wants to optimize. A *property* here is the string of the parameter displayed in the GenericObjectEditor (generated though Introspection), e.g., `bagSizePercent` or `classifier` of `weka.classifiers.meta.Bagging`.

Due to some important bugfixes, one should obtain a version of Weka >3.5.6 later than 11 Sept 2007.

For each of the two axes, X and Y, one can specify the following parameters:

* **property**
> The dot-separated path pointing to the property to be optimized. In order to distinguish between paths for the filter or the classifier, one needs to prefix the path either with `filter.` or `classifier.` for filter or classifier path respectively.
* **expression**
> The mathematical expression to generate the value for the property, processed with the `weka.core.MathematicalExpression` class, which supports the following functions: `abs`, `sqrt`, `log`, `exp`, `sin`, `cos`, `tan`, `rint`, `floor`, `pow`, `ceil`. These variables are available in the expression: 
`BASE`, `FROM`, `TO`, `STEP`, `I`; with `I` ranging from `FROM` to `TO`.
* **min**
> The minimum value to start from.
* **max**
> The maximum value.
* **step**
> The step size used to get from *min* to *max*.
* **base**
> Used in `pow()` calculations.

`GridSearch` can also optimized based on the following measures:

* Correlation coefficient (= CC)
* Root mean squared error (= RMSE)
* Root relative squared error (= RRSE)
* Mean absolute error (= MAE)
* Root absolute error (= RAE)
* Combined: 
(1-abs(CC)) + RRSE + RAE
* Accuracy (= ACC)
* Kappa (= KAP) [only when using Weka packages]

Note: 
*Correlation coefficient* is only available for numeric classes and *Accuracy* only for nominal ones.

Here are a some examples (taken from the Javadoc of the classifier):

* **Optimizing SMO with RBFKernel (C and gamma)**
	* Start the Explorer and load your dataset with nominal class.
	* Set the evaluation to *Accuracy*.
	* Set the filter to `weka.filters.AllFilter` since we don't need any special data processing and we don't optimize the filter in this case (data gets always passed through filter!).
	* Set `weka.classifiers.functions.SMO` as classifier with `weka.classifiers.functions.supportVector.RBFKernel` as kernel.
	* Set the`XProperty` to "classifier.c", `XMin` to "1", `XMax` to "16", `XStep` to "1" and the `XExpression` to "I". This will test the "C" parameter of `SMO` for the values from 1 to 16.
	* Set the `YProperty` to "classifier.kernel.gamma", `YMin` to "-5", `YMax` to "2", `YStep` to "1", `YBase` to "10" and `YExpression` to "pow(BASE,I)". This will test the gamma of the RBFKernel with the values 10<sup>-5</sup>, 10<sup>-4</sup>,..,10<sup>2</sup>.
	* Output will be similar to this one here:

```
 Filter: 
weka.filters.AllFilter
 Classifier: 
weka.classifiers.functions.SMO -C 2.0 -L 0.0010 -P 1.0E-12 -N 0 -V -1 -W 1 -K "weka.classifiers.functions.supportVector.RBFKernel -C 250007 -G 0.0"

 X property: 
classifier.c
 Y property: 
classifier.kernel.gamma

 Evaluation: 
Accuracy
 Coordinates: 
[2.0, 0.0]
 Values: 
**2.0** (X coordinate), **1.0** (Y coordinate)
```
* **Optimizing PLSFilter with LinearRegression (# of components and ridge)** - default setup
	* Start the Explorer and load your dataset with numeric class.
	* Set the evaluation to Correlation coefficient.
	* Set the filter to `weka.filters.supervised.attribute.PLSFilter`.
	* Set `weka.classifiers.functions.LinearRegression` as classifier and use *no attribute selection* and *no elimination of colinear attributes* (speeds up `LinearRegression` significantly!).
	* Set the `XProperty` to "filter.numComponents", `XMin` to "5", `XMax` to "20" (this depends heavily on your dataset, should be no more than the number of attributes!), `XStep` to "1" and `XExpression` to "I". This will test the number of components the `PLSFilter` will produce from 5 to 20.
	* Set the `YProperty` to "classifier.ridge", `XMin` to "-10", `XMax` to "5", `YStep` to "1" and `YExpression` to "pow(BASE,I)". This will try ridge parameters from 10<sup>-10</sup> to 10<sup>5</sup>.
	* Output will be similar to this one:

```
 Filter: 
weka.filters.supervised.attribute.PLSFilter -C 5 -M -A PLS1 -P center
 Classifier: 
weka.classifiers.functions.LinearRegression -S 1 -C -R 5.0

 X property: 
filter.numComponents
 Y property: 
classifier.ridge

 Evaluation: 
Correlation coefficient
 Coordinates: 
[5.0, 5.0]
 Values: 
**5.0** (X coordinate), **100000.0** (Y coordinate)
```
**Notes:**

* a property for the classifier starts with **classifier.**
* a property for the filter starts with **filter.**
* Arrays of objects are addressed with **[<index>]**, with the *index* being 0-based. E.g., using a `weka.filters.MultiFilter` in `GridSearch` consisting of a `ReplaceMissingValues` and a `PLSFilter` filter one can address the *numComponents* property of the `PLSFilter` with *filter.filter[1].numComponents*

# MultiSearch 
`weka.classifiers.meta.MultiSearch` is available through [this](https://github.com/fracpete/multisearch-weka-package) Weka package (requires Weka 3.7.11 or later; for downloads see the *Releases* section). MultiSearch is similar to GridSearch, more general and simpler at the same time. More general, because it allows the optimization of an arbitrary number of parameters, not just two. Simpler, because it does not offer any search space expansions or [gnuplot](http://gnuplot.info/) output and less options.

For each parameter to optimize, the user has to define a *search parameter*. There are two types of parameters available:

* `MathParameter` - basically what `GridSearch` uses, with an expression to calculate the actual value using the min, max and step parameters
* `ListParameter` - the blank-separated list of values is used as input for the optimization (useful, if values cannot be described by a mathematical function)

Here is a setup for finding the best ridge parameter (property `classifier.ridge`) using the `MathParameter` search parameter using values from 10^-10 to 10^5:

```
weka.classifiers.meta.MultiSearch \
  -E CC \
  -search "weka.core.setupgenerator.MathParameter -property classifier.ridge -min -10.0 -max 5.0 -step 1.0 -base 10.0 -expression pow(BASE,I)" \
  -sample-size 100.0 -initial-folds 2 -subsequent-folds 10 -num-slots 1 -S 1 \
  -W weka.classifiers.functions.LinearRegression -- -S 1 -C -R 1.0E-8
```

And here using the `ListParameter` search parameter for evaluating values 0.001, 0.05, 0.1, 0.5, 0.75 and 1.0 for the ridge parameter (property `classifier.ridge`):

```
weka.classifiers.meta.MultiSearch \
  -E CC \
  -search "weka.core.setupgenerator.ListParameter -property classifier.ridge -list \"0.001 0.05 0.1 0.5 0.75 1.0\"" \
  -sample-size 100.0 -initial-folds 2 -subsequent-folds 10 -num-slots 1 -S 1 \
  -W weka.classifiers.functions.LinearRegression -- -S 1 -C -R 1.0E-8
```

`MultiSearch` can be optimized based on the following measures:

* Correlation coefficient (= CC)
* Root mean squared error (= RMSE)
* Root relative squared error (= RRSE)
* Mean absolute error (= MAE)
* Root absolute error (= RAE)
* Combined: 
(1-abs(CC)) + RRSE + RAE
* Accuracy (= ACC)
* Kappa (= KAP)

# Auto-WEKA 

[Auto-WEKA](https://www.cs.ubc.ca/labs/beta/Projects/autoweka/) is available as a package through the WEKA package manager. It provides the class `weka.classifiers.meta.AutoWEKAClassifier` and optimizes all parameters of all learners. It also automatically determines the best learner to use and the best attribute selection method for a given dataset. More information is available on the project website and the [manual](https://www.cs.ubc.ca/labs/beta/Projects/autoweka/manual.pdf).

# Downloads 
* [CVParam.java](files/CVParam.java) - optimizes J48's `-C` parameter

# See also 
* [LibSVM](lib_svm.md) - you need additional jars in your [CLASSPATH](classpath.md) to be able to use LibSVM

# Links 
* [gnuplot homepage](http://www.gnuplot.info/)
* [Java Beans](http://java.sun.com/docs/books/tutorial/javabeans/)
* [Introspection](http://java.sun.com/docs/books/tutorial/javabeans/introspection/)