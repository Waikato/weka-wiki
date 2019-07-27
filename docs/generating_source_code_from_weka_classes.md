

Some of the schemes in Weka can generate Java source code that represents their current internal state. At the moment these are classifiers (book and developer version) and filters ([snapshot](snapshots.md) or >3.5.6). The generated code can be used within Weka as normal classifier/filter, since this code will be derived from the same superclass (`weka.classifiers.Classifier` or `weka.filters.Filter`) as the generating code.

**Note:** The commands listed here are for a Linux/Unix bash (the backslash tells the shell that the command isn't finished yet and continues on the next line). In case of Windows or the SimpleCLI, just remove the backslashes and put everything on one line.

# Classifiers
Instead of using a serialized filter to perform further classifications/predictions, one can also obtain source code from a trained classifier and use this instead. The advantage of this is being less dependent on version changes and incompatible serialized files. All classifiers implementing the `weka.classifiers.Sourcable` interface can turn their model into Java source code (check the Javadoc of this interface for all the classifiers implementing it).

Here's an example of generating source code from a trained `J48` (the source code is saved in a file called `WekaWrapper.java`):

```bash
 java weka.classifiers.trees.J48 \
   -t /some/where/data.arff \
   -z SourcedJ48 \                  # name of the inner class, gets called by wrapper class WekaWrapper
   > /else/where/WekaWrapper.java   # redirecting the output of the code into a file
```
The package of the wrapper class is by default the `weka.classifiers` package. Make sure that you place the source code and/or class files in the correct location. The generated classifier can be used from the commandline or GUI like any other classifier within Weka, you only need to make sure that your [GenericObjectEditor](generic_object_editor.md) lists the package you place the classifier in (`weka.classifiers` is **not** listed by default).

The following command calls the generated classifier with a training set (training has no effect, of course) and outputs the predictions for this dataset to `stdout`:

```bash
 java weka.classifiers.WekaWrapper \
   -t /some/file.arff \
   -p 0                  # output predictions for training set
```
**Note:** the Explorer can output source code as well, you only have to check the *Output source code* option in the *More options* dialog.

# Filters
With versions of Weka later than 3.5.6 or a [snapshot](snapshots.md) of the developer version, one can now also turn filters into source code. The process is basically the same as with classifiers outlined above. All filters that implement the `weka.filters.Sourcable` interface can be turned into Java code (again, check out the Javadoc for this interface, to see the filters implementing it).

The following command turns an initialized ReplaceMissingValues filter into source code:

```bash
 java weka.filters.unsupervised.attribute.ReplaceMissingValues \
   -i /somewhere1/input.arff \
   -o /somewhere2/output.arff \
   -z SourcedRMV \                   # name of the inner class, gets called by wrapper class WekaWrapper
   > /some/place/WekaWrapper.java    # redirecting the output of the code into a file
```
The package of the wrapper class is by default the `weka.filters` package. Make sure that you place the source code and/or class files in the correct location. The generated filter can be used from the commandline or GUI like any other filter within Weka, you only need to make sure that your [GenericObjectEditor](generic_object_editor.md) lists the package you place the filter in.

And again a little demonstration of how to call the generated source code:

```bash
 java weka.filters.WekaWrapper \
   -i /some/where/input.arff \     # must have the same structure as **/somewhere1/input.arff**, of course
   -o /other/place/output.arff 
```

# See also
* [Serialization](serialization.md) - can be used for all classifiers and filters to save them in a persistent state.
