Multi-instance (MI) classification is a supervised learning technique, but differs from *normal* supervised learning:

* it has multiple instances in an example
* only one class label is observable for all the instances in an example

# Classifiers
Multi-instance classifiers were originally available through a separate software package, [Multi-Instance Learning Kit](http://www.cs.waikato.ac.nz/~ml/milk/) (= MILK). Weka handles relational attributes now natively since 3.5.3 and the multi-instance classifiers are available through the [multiInstanceLearning](https://weka.sourceforge.io/doc.packages/multiInstanceLearning/) package. Once the package has been installed, the classifiers can be found in the following package:

```text
 weka.classifiers.mi
```

# Data format
The data format for multi-instance classifiers is fairly simple:

* **bag-id** - nominal attribute; unique identifier for each bag
* **bag** - relational attribute; contains the instances of an example
* **class** - the class label for the examples

Weka offers two filters to convert from flat file format (or propositional format), which is normally used in supervised classification, to multi-instance format and vice versa:

* `weka.filters.unsupervised.attribute.PropositionalToMultiInstance`
* `weka.filters.unsupervised.attribute.MultiInstanceToPropositional`

Here is an example of the *musk1* UCI dataset, used quite often in publications covering MI learning (Note: **...** denotes omission):

* **propositional format:**
> This [ARFF](formats_and_processing/arff.md) file lists all the attributes, *molecule_name* (which is the bag-id), *f1* to *f166* (containing the actual data of the instances) and the *class* attribute.
```text
 @relation musk1
 
 @attribute molecule_name {MUSK-jf78,MUSK-jf67,MUSK-jf59,...,NON-MUSK-199}
 @attribute f1 numeric
 @attribute f2 numeric
 @attribute f3 numeric
 @attribute f4 numeric
 @attribute f5 numeric
 ...
 @attribute f166 numeric
 @attribute class {0,1}
 
 @data
 MUSK-188,42,-198,-109,-75,-117,11,23,-88,-28,-27,...,48,-37,6,30,1
 MUSK-188,42,-191,-142,-65,-117,55,49,-170,-45,5,...,48,-37,5,30,1
 ...
```
* **multi-instance format:**
> Using the relational attribute, one only has *three* attributes on the first level: 
*molecule_name*, *bag* and *class*. The relational attribute contains the instances for each example, consisting of the attributes *f1* to *f166*. The data of the relational attribute is surrounded by quotes and the single instances inside the bag are separated by line-feeds (= `\n`).
```text
 @relation musk1
 
 @attribute molecule_name {MUSK-jf78,MUSK-jf67,MUSK-jf59,...,NON-MUSK-199}
 @attribute bag relational
   @attribute f1 numeric
   @attribute f2 numeric
   @attribute f3 numeric
   @attribute f4 numeric
   @attribute f5 numeric
   ...
   @attribute f166 numeric
 @end bag
 @attribute class {0,1}
 
 @data
 MUSK-188,"42,-198,-109,-75,-117,11,23,-88,-28,-27,...,48,-37,6,30\n42,-191,-142,-65,-117,55,49,-170,-45,5,...,48,-37,5,30\n...",1
 ...
```

# See also
* [Use Weka in your Java code](use_weka_in_your_java_code.md) - general article about using the Weka API
* [Creating an ARFF file](formats_and_processing/creating_arff_file.md) - explains how to create an ARFF file from within Java, incl. relational attributes

# Links
* Xin Xu. *Statistical learning in multiple instance problem.* Master's thesis, University of Waikato, Hamilton, NZ, 2003. 0657.594. [Download](http://www.cs.waikato.ac.nz/~ml/publications/2003/xinxu_thesis.ps.gz)
* [MILK homepage](http://www.cs.waikato.ac.nz/~ml/milk/)
* [multiInstanceLearning Javadoc](https://weka.sourceforge.io/doc.packages/multiInstanceLearning/)
