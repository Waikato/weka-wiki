The filter `RemoveFolds` (package `weka.filters.unsupervised.instance`) can be used to generate the train/test splits used in cross-validation (for stratified folds, use `weka.filters.supervised.instance.StratifiedRemoveFolds`). The filter has to be used twice for each train/test split, first to generate the train set and then to obtain the test set.

Since this is rather cumbersome by hand, one can also put this into a [bash](http://en.wikipedia.org/wiki/Bash) script:

```bash
 #!/bin/bash
 #
 # expects the weka.jar as first parameter and the datasets to work on as 
 # second parameter.
 #
 # FracPete, 2007-04-10
 
 if [ ! $# -eq 2 ]
 then
   echo
   echo "usage: folds.sh <weka.jar> <dataset>"
   echo
   exit 1
 fi
 
 JAR=$1
 DATASET=$2
 FOLDS=10
 FILTER=weka.filters.unsupervised.instance.RemoveFolds
 SEED=1
 
 for ((i = 1; i <= $FOLDS; i++))
 do
   echo "Generating pair $i/$FOLDS..."
 
   OUTFILE=`echo $DATASET | sed s/"\.arff"//g`
   # train set
   java -cp $JAR $FILTER -V -N $FOLDS -F $i -S $SEED -i $DATASET -o "$OUTFILE-train-$i-of-$FOLDS.arff"
   # test set
   java -cp $JAR $FILTER    -N $FOLDS -F $i -S $SEED -i $DATASET -o "$OUTFILE-test-$i-of-$FOLDS.arff"
 done
```

The script expects two parameters:

1. the `weka.jar` (or the path to the Weka classes)
2. the dataset to generate the train/test pairs from 

Example:

```bash
 ./folds.sh /some/where/weka.jar /some/where/else/dataset.arff
```

This example will create the train/test splits for a 10-fold cross-validation at the same location as the original dataset, i.e., in the directory `/some/where/else/`.

# Downloads

* [folds.sh](files/folds.sh)
