

Unless one has access to a 64-bit machine with lots of RAM, it can happen quite easy that one runs into an [OutOfMemoryException](faqs/OutOfMemoryException.md) running WEKA on large datasets. This article tries to present some solutions apart from buying new hardware.

# Sampling
The question is, does one really need to train with **all** the data, or is a subset of the data already sufficient? 

WEKA offers several filters for re-sampling a dataset and generating a new dataset reduced in size:

* `weka.filters.supervised.instance.Resample`
> This filter takes the class distribution into account for generating the sample, i.e., you can even adjust the distribution by adding a bias.
* `weka.filters.unsupervised.instance.Resample`
> The unsupervised filter does not take the class distribution into account for generating the output.
* `weka.filters.supervised.instance.SpreadSubsample`
> It allows you to specify the maximum "spread" between the rarest and most common class.

See the respective Javadoc for more information ([book version](http://weka.sourceforge.net/doc.stable/), [developer version](http://weka.sourceforge.net/doc.dev/)).

# Incremental classifiers
Most classifiers need to see all the data before they can be trained, e.g., J48 or SMO. But there are also schemes that can be trained in an incremental fashion, not just in batch mode. All classifiers implementing the `weka.classifiers.UpdateableClassifier` interface are able to process data in such a way.

Running such a classifier from commandline will load the dataset incrementally (NB: 
not all data formats can be loaded incrementally; [XRFF](xrff.md) is one of them, [ARFF](arff.md) on the other hand can be read incrementally) and feed the data instance by instance to the classifier.

Check out the Javadoc of the `UpdateableClassifier` interface to see what schemes implement it ([book version](http://weka.sourceforge.net/doc.stable/weka/classifiers/UpdateableClassifier.html), [developer version](http://weka.sourceforge.net/doc.dev/weka/classifiers/UpdateableClassifier.html)).

# Other tools
* [MOA - Massive Online Analysis](http://sourceforge.net/projects/moa-datastream/)
> A framework for learning from a continuous supply of examples, a data stream. Includes tools for evaluation and a collection of machine learning algorithms. Related to the WEKA project, also written in Java, while scaling to more demanding problems.
