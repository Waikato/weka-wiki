People often want to **tag** their **instances with identifiers**, so they can keep track of them and the predictions made on them.

## Adding the ID
A new ID attribute is added real easy: one only needs to run the `AddID` filter over the dataset and it's done. Here's an example (at a DOS/Unix command prompt):

```bash
 java weka.filters.unsupervised.attribute.AddID
   -i data_without_id.arff
   -o data_with_id.arff
```
(all on a single line)

**Note:** the AddID filter adds a numeric attribute, not a String attribute to the dataset. If you want to remove this ID attribute for the classifier in a `FilteredClassifier` environment again, use the `Remove` filter instead of the `RemoveType` filter (same package).

## Removing the ID
If you run from the command line you can use the `-p` option to output predictions plus any other attributes you are interested in. So it is possible to have a string attribute in your data that acts as an identifier. A problem is that most classifiers don't like String attributes, but you can get around this by using the `RemoveType` (this removes *String* attributes by default).

Here's an example. Lets say you have a training file named `train.arff`, a testing file named `test.arff`, and they have an identifier String attribute as their 5th attribute. You can get the predictions from `J48` along with the identifier strings by issuing the following command (at a DOS/Unix command prompt):

```bash
 java weka.classifiers.meta.FilteredClassifier
   -F weka.filters.unsupervised.attribute.RemoveType
   -W weka.classifiers.trees.J48
   -t train.arff -T test.arff -p 5
```
(all on a single line)

If you want, you can redirect the output to a file by adding "` > output.txt`" to the end of the line.

In the Explorer GUI you could try a similar trick of using the String attribute identifiers here as well. Choose the `FilteredClassifier`, with the `RemoveType` as the filter, and whatever classifier you prefer. When you visualize the results you will need click through each instance to see the identifier listed for each.
