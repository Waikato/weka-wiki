This article discusses the use of cluster schemes in Weka from the commandline. This functionality is, of course, also available from the GUI, namely the Explorer and the KnowledgeFlow.

# Commandline
Clusterers can be used in a similar fashion Weka's classifiers:

* `-t <file>`
> specifies the training file
* `-T <file>`
> specifies the test file
* `-p <attribute range>`
> for outputting predictions (if a test file is present, then for this one, otherwise the train file)
* `-c <index>`
> performs a *classes to clusters* evaluation (during training, the class attribute will be automatically ignored)
* `-x <folds>`
> performs cross-validation for density-based clusterers (no *classes to clusters* evaluation possible!). With `weka.clusterers.MakeDensityBasedClusterer`, any clusterer can be turned into a density-based one.
* `-d <file> and -l <file>`
> for saving and loading [serialized models](serialization.md)

Some examples:

* `EM` with train and test file:

```bash
 java weka.clusterers.EM \
    -I 10 \             # only 10 iterations
    -t train.arff \
    -T test.arff
```
* `SimpleKMeans` with *classes to clusters* evaluation:

```bash
 java weka.clusterers.SimpleKMeans 
   -t train.arff \
   -c last             # the class attribute is the last
```
>  Sample output:

```text
 ...
 Class attribute: 
class
 Classes to Clusters:

 
    0   1  <-- assigned to cluster
  242 442 | 3
   22  77 | 2
 
 Cluster 0 <-- 2
 Cluster 1 <-- 3
 
 Incorrectly clustered instances : 
      319.0    40.7407 %
```
* running 2-fold cross-validation on `SimpleKMeans` (we need to use `MakeDensityBasedClusterer`, since `SimpleKMeans` is no density-based clusterer!):

```bash
 java weka.clusterers.MakeDensityBasedClusterer 
    -W weka.clusterers.SimpleKMeans \
    -t train.arff \
    -x 2                # 2 folds
```
>  Sample output:

```text
 ...
 2 fold CV Log Likelihood: 
-40.9751
```

# Filters
Weka contains some filters that make life easier with the cluster algorithms.

## AddCluster
The filter `weka.filters.unsupervised.attribute.AddCluster` adds the cluster number as nominal attribute to the data processed by the filter. This makes the post-processing or analyzing of the cluster assignments easier than with the `-p X` option.

Here's an example for the UCI [dataset](datasets.md) *anneal* using `SimpleKMeans`:

```bash
 java weka.filters.unsupervised.attribute.AddCluster \
    -W "weka.clusterers.SimpleKMeans -N 6 -S 42" \
    -I last \                 # we want to ignore the class attribute
    -i anneal.arff \
    -o out.arff
```
And some example output:

```text
 @relation ...
 
 @attribute family {'?',GB,GK,GS,TN,ZA,ZF,ZH,ZM,ZS}
 @attribute product-type {C,H,G}
 ...
 @attribute class {1,2,3,4,5,U}
 @attribute cluster {cluster1,cluster2,cluster3,cluster4,cluster5,cluster6}
 
 @data
 '?',C,A,8,0,'?',S,'?',0,'?','?',G,'?','?','?','?','?','?','?','?','?','?','?','?','?','?','?','?','?','?','?',COIL,0.7,610,0,'?',0,'?',3,cluster2
 '?',C,R,0,0,'?',S,2,0,'?','?',E,'?','?','?','?','?','?','?','?','?','?','?','?','?','?','?',Y,'?','?','?',COIL,3.2,610,0,'?',0,'?',3,cluster2
```

## ClusterMembership
If you're more interested in the probability for each each cluster an instance gets assigned, you can use the filter `weka.filters.unsupervised.attribute.ClusterMembership`.

Here's an example for the UCI [dataset](datasets.md) *anneal* using `EM`:

```bash
 java weka.filters.unsupervised.attribute.ClusterMembership \
    -W weka.clusterers.EM \
    -I last \                 # we want to ignore the class attribute
    -i anneal.arff \
    -o out.arff \
    -- \                      # additional options for EM follow after the --
    -I 10
```
And some example output:

```text
 @relation ...
 
 @attribute pCluster_0_0 numeric
 @attribute pCluster_0_1 numeric
 @attribute pCluster_0_2 numeric
 @attribute pCluster_0_3 numeric
 @attribute pCluster_0_4 numeric
 @attribute pCluster_0_5 numeric
 
 @data
 0.000147,0.009863,0,0.98999,0.000001,0
 0.00292,0.000002,0,0.997078,0,0
 ...
```

# Classifiers

## ClassificationViaClustering

A new meta-classifier, `weka.classifiers.meta.ClassificationViaClustering`, got introduced in the developer version (>3.5.6 or [snapshot](snapshots.md)), which mimics the *clusters to classes* functionality of the `weka.core.ClusterEvaluation` class. A user defined cluster algorithm is built with the training data presented to the meta-classifier (after the class attribute got removed, of course) and then the mapping between classes and clusters is determined. This mapping is then used for predicting class labels of unseen instances.

Here's an example for the UCI [dataset](datasets.md) *balance-scale*:

```bash
 java weka.classifiers.meta.ClassificationViaClustering \
    -t balance-scale.arff \
    -W weka.clusterers.SimpleKMeans \
    -- \
    -N 3           # additional parameters for SimpleKMeans, since the dataset has 3 class labels
```
And some sample output:

```text
 ...
 Clusters to classes mapping:

   1. Cluster: 
B (2)
   2. Cluster: 
R (3)
   3. Cluster: 
L (1)
 
 Classes to clusters mapping:

   1. Class (L): 
3. Cluster
   2. Class (B): 
1. Cluster
   3. Class (R): 
2. Cluster
 ...
```
**Note:** In order to obtain a useful model, you have to make sure that the cluster algorithms are properly setup for the dataset you are using. E.g., `SimpleKMeans` has a fixed number of clusters that it should determine. Trying to determine 2 clusters on a dataset with 5 class labels isn't very useful.

# Notes

* All examples are for a Linux bash. For Windows or the SimplCLI, just remove the backslashes and collapse all the lines into a single one.
* Comments in the examples follow a **#** and need to be removed, of course.
* **...** denotes an omission of unnecessary content.

# See also

* [Use Weka in your Java code](use_weka_in_your_java_code.md), section *Clustering* explains using the Weka API for clusterers
* [Batch filtering](batch_filtering.md) - shows how to use filters in batch mode
* [Serialization](serialization.md) - for using serialized/saved models
