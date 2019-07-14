# Notes

* This bug has now been fixed. (12/2014)
    * There is a bug in the code to build a library -- trying to build any model specification with three layers (e.g., Bagging a REPTree) causes the form to freeze up and/or crash.
* The documentation on how to run from the command line is outdated. Some corrections:
    * The "-D" option no longer exists.
    * The command shown for training a library from the command line:

    > ```java weka.classifiers.meta.EnsembleSelection -no-cv -v -L path/to/your/mode/list/file.model.xml -W /path/to/your/working/directory -A library -X 5 -S 1 -O -t yourTrainingInstances.arff```

    > fails for me with an exception that "Folds 1 and 5 are not equal." A command line that works is to set the folds to 1:

    > ```java weka.classifiers.meta.EnsembleSelection -no-cv -v -L path/to/your/mode/list/file.model.xml -W /path/to/your/working/directory -A library -X 1 -S 1 -O -t yourTrainingInstances.arff```

# Links

* [Ensemble_selection.pdf](files/Ensemble_selection.pdf) - Documentation on how to use Ensemble Selection in Weka
* [Ensemble Selection from Libraries of Models, ICML'04](http://www.cs.cornell.edu/~caruana/caruana.icml04.revised.rev2.ps)