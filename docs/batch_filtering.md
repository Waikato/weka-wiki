*Batch filtering* is used if a second dataset, normally the test set, needs to be processed with the same statistics as the the first dataset, normally the training set.

For example, performing standardization with the `Standardize` filter on two datasets separately will most certainly create two differently standardized output files, since the mean and the standard deviation are based on the input data (and those will differ if the datasets are different). The same applies to the `StringToWordVector`: here the word dictionary will change, since word occurrences will differ in training and test set. The generated output will be two incompatible files.

In order to create compatible train and test set, batch filtering is necessary. Here, the first input/output pair (`-i`/`-o`) initializes the filter's statistics and the second input/output pair (`-r`/`-s`) gets processed according to those statistics. To enable batch filtering, one has to provide the additional parameter **`-b`** on the commandline.

Here is an example Java call:

```
 java weka.filters.unsupervised.attribute.Standardize \
   -b \
   -i train.arff \
   -o train_std.arff \
   -r test.arff \
   -s test_std.arff
```

**Note:** The commandline outlined above is for a Linux/Unix bash (the backslash tells the shell that the command isn't finished yet and continues on the next line). In case of Windows or the SimpleCLI, just remove those backslashes and put everything on one line.

# See also
* See section [Batch filtering](use_weka_in_your_java_code.md#batch-filtering) in the article [Use Weka in your Java code](use_weka_in_your_java_code.md), in case you need to perform batch filtering from within your own code
