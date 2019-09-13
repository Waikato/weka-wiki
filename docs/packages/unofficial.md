There are a number of packages for WEKA 3.8 on the internet that are not listed in the "official" WEKA package repository. These packages can nevertheless be easily installed via the package manager in WEKA 3.8 (available via the Tools menu in WEKA's GUIChooser) by providing the URL for the package .zip file.

Below is an (incomplete list) of packages that are available.
 
# Preprocessing

* [data-dumper](https://github.com/fracpete/data-dumper-weka-package) -- meta-filter/-classifier/-clusterer that allow listening to the data being used, eg within filter pipelines (aka MultiFilter).
* [dataset-weights](https://github.com/fracpete/dataset-weights-weka-package) -- filters for setting attribute and instance weights using various methods.
* [missing-values-imputation](https://github.com/fracpete/missing-values-imputation-weka-package) -- various methods for imputing missing values using a filter.
* [mxexpression](https://github.com/fracpete/mxexpression-weka-package) -- filter for updating a target attribute using a mathematical expression.

# Classification

* [Java neural network package](https://github.com/amten/NeuralNetwork) -- Java (convolutional or fully-connected) neural network implementation with plugin for [Weka](http://www.cs.waikato.ac.nz/ml/weka/). Uses dropout and rectified linear units. Implementation is multithreaded and uses [MTJ](https://github.com/fommil/matrix-toolkits-java) matrix library with native libs for performance.
* [HMMWeka](http://doc.gold.ac.uk/~mas02mg/software/hmmweka/) -- This library makes Hidden Markov Model machine learning available in Weka.
* [Collective classification](https://github.com/fracpete/collective-classification-weka-package) -- Algorithms around semi-supervised learning and collective classification.
* [Bagging ensemble selection](http://www.quansun.com/bagging_es/) -- Bagging Ensemble Selection - a new ensemble learning strategy.
* [DataSqueezer](http://www.cioslab.vcu.edu/alg/InstallWekaPackage.htm) -- Efficient rule builder that generates a set of production rules from labeled input data. It can handle missing data and has log-linear asymptotic complexity with the number of training examples.
* [miDS](http://www.cioslab.vcu.edu/alg/InstallWekaPackage.htm) -- mi-DS is a multiple-Instance learning supervised algorithm based on the DataSqueezer algorithm.
* [LibD3C](http://datamining.xmu.edu.cn/main/~chenwq/downloads/) -- Ensemble classifiers with a clustering and dynamic selection strategy.
* [ICRM](http://www.uco.es/grupos/kdis/kdiswiki/ICRM/) -- An Interpretable Classification Rule Mining Algorithm.
* [tclass](https://github.com/fracpete/tclass-weka-package) -- TClass is a supervised learner for multivariate time series, originally developed by [Waleed Kadous](http://www.cse.unsw.edu.au/~waleed/).
* [wekaclassalgos](https://github.com/fracpete/wekaclassalgos) -- collection of artificial neural network (ANN) algorithms and artificial immune system (AIS) algorithms, originally developed by [Jason Brownlee](https://sourceforge.net/projects/wekaclassalgos/).
* [mxexpression](https://github.com/fracpete/mxexpression-weka-package) -- classifier for making predictions using a mathematical expression.

# Clustering

* [APCluster](http://datamining.xmu.edu.cn/main/~chenwq/downloads) -- Affinity propagation algorithm for clustering, used especially in bioinformatics and computer vision.
* [Fast Optics](http://voltaic-sandbox-523.appspot.com/projects.html) -- Fast Implementation of OPTICS algorithm using random projections for Euclidean distances.

# Similarity functions

* [wekabiosimilarity](http://sourceforge.net/projects/wekabiosimilarity/) -- implements several measures to compare binary feature vectors; and, additionally, extrapolates those measures to work with multi-value, string and numerical feature vectors.

# Discretization

* [ur-CAIM](http://www.uco.es/grupos/kdis/kdiswiki/ur-CAIM/) -- Improved CAIM Discretization for Unbalanced and Balanced Data.
* [CAIM](http://www.cioslab.vcu.edu/alg/InstallWekaPackage.htm) -- Class-Attribute Interdependence Maximization algorithm: discretizes a continuous feature into a number of intervals. This is done by using class information, without requiring the user to provide this number.

# Feature selection

* [RSARSubsetEval](http://users.aber.ac.uk/pds7/weka/) -- Rough set feature selection.

# Frequent pattern mining

* [XApriori](https://github.com/mniemann87/XApriori) --Available case analysis modification of Apriori frequent pattern mining algorithm.

# Stemming

* [Snowball stemmers](https://github.com/fracpete/snowball-stemmers-weka-package/releases) -- Contains the actual snowball stemmer algorithms to make the Snowball stemmer wrapper in Weka work.
* [PTStemmer](https://github.com/fracpete/ptstemmer-weka-package/releases) -- Wrapper for [Pedro Oliveira's stemmer library](http://code.google.com/p/ptstemmer/) for Portuguese.

# Text mining

* [nlp](https://github.com/fracpete/nlp-weka-package/releases) -- Contains components for natural language processing, eg part-of-speech tagging filter and Penn Tree Bank tokenizer. Makes use of the [Stanford Parser](http://nlp.stanford.edu/software/) (parser models need to be downloaded separately).

# Visualization

* [graphviz-treevisualize](https://github.com/fracpete/graphviz-treevisualize-weka-package/releases) -- Generating nice graphs in the Explorer from trees (eg J48) using the GraphViz executables.
* [confusionmatrix](https://github.com/fracpete/confusionmatrix-weka-package/releases) -- Various visualizations of confusion matrices in the Explorer.
* [serialized-model-viewer](https://github.com/fracpete/serialized-model-viewer-weka-package/releases) -- Adds a standalone tab to the Explorer that allows the user to load a serialized model and view its content as text (simply uses the objects' `toString()` method).

# Parameter optimization

* [multisearch](https://github.com/fracpete/multisearch-weka-package/releases) -- Meta-classifier similar to GridSearch, but for optimizing arbitrary number of parameters.

# Others

* [screencast4j](https://github.com/fracpete/screencast4j-weka-package) -- Allows you to record sound, webcam and screen feeds, storing them in separate files to be combined into a screencast using a [video editor](https://en.wikipedia.org/wiki/List_of_video_editing_software). This screencast you can then share on YouTube, for instance.
* [command-to-code](https://github.com/fracpete/command-to-code-weka-package) -- Turns command-lines (eg of classifiers or filters) into various Java code snippets.
* [jshell-scripting](https://github.com/fracpete/jshell-scripting-weka-package) -- Allows scripting in Java, using [jshell](https://docs.oracle.com/javase/9/jshell/)

