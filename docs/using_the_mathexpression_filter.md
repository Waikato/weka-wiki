The filter `MathExpression` can be found in this package:

```text
 weka.filters.unsupervised.attribute
```
It provides a powerful means of performing mathematical transformations of numeric attributes. The following operators are supported:

```text
 +, -, *, /, (, ), 
 pow, log, abs, cos, exp, sqrt, tan, sin, ceil, floor, rint, 
 MEAN, MAX, MIN, SD, COUNT, SUM, SUMSQUARED, ifelse
```
The attribute value that is being processed, can be referenced as **`A`**.

# Manual discretization
One can even use the filter for manually discretizing numeric attributes, if the other `Discretize` filters (supervised and unsupervised) cannot be used. This works thanks to the `ifelse` operator. 

It is basically a two-step-process:

* run `MathExpression` to turn all the values into discrete ones
* run `NumericToNominal` to turn the numeric values then into nominal labels

Here's an example:

* a dataset where the first attribute needs to be discretized into 3 bins
* the bins need to be as follows

```text
 (-inf-20.0]
 (20.0-80.0]
 (80.0-inf)
```

* using `MathExpression` to create discrete values

```bash
 weka.filters.unsupervised.attribute.MathExpression \
   -E "ifelse(A>20, ifelse(A>80, 3, 2), 1)" \
   -V \
   -R 1
```

> **Note:** `-V -R 1` means we only want to transform the first attribute. Without `-V` all the numeric attributes would be transformed according to this expression.
* this results in the following transformation

```text
 (-inf-20.0] -> 1
 (20.0-80.0] -> 2
 (80.0-inf)  -> 3
```

* using `NumericToBinary` to create a nominal attribute from the numeric one

```bash
 weka.filters.unsupervised.attribute.NumericToNominal \
   -R 1
```

* *optional:* if one wants to rename those labels, one can use the class listed in the [Rename Attribute Values](formats_and_processing/rename_attribute_values.md) article for that

**Note:** the "\" at the end of the lines tell a *nix [bash](http://en.wikipedia.org/wiki/bash) to continue on the next line.
