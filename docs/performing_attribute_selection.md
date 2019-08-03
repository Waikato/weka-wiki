

In Weka, you have three options of performing attribute selection from commandline (not everything is possible from the GUI):

* the [**native**](performing_attribute_selection.md#native) approach, using the attribute selection classes directly
* using a [**meta-classifier**](performing_attribute_selection.md#meta-classifier)
* the [**filter**](performing_attribute_selection.md#filter) approach

**Notes:**

* The commandlines outlined in this article are for a Linux/Unix bash (the backslash tells the shell that the command isn't finished yet and continues on the next line). In case of Windows or the SimpleCLI, just remove those backslashes and put everything on one line.
* The Explorer in the developer version (>= 3.5.4) also outputs the commandline setups to its log. Just click on the *Log* button to display the log and copy/paste the commandlines (you will need to add the appropriate `java` call and dataset files, of course).

# Native
Using the attribute selection classes directly outputs some additional useful information, like number of subsets evaluated/best merit (for subset evaluators), ranked output with merit per attribute (for ranking based setups). The attribute selection classes are located in the following package:

```text
 weka.attributeSelection
```
Example using `CfsSubsetEval` and `BestFirst`:

```bash
 java weka.attributeSelection.CfsSubsetEval \
    -M \
    -s "weka.attributeSelection.BestFirst -D 1 -N 5" \
    -i <file.arff>
```

# Meta-classifier
Weka also offers a meta-classifier that takes a search algorithm and evaluator next to the base classifier. This makes the attribute selection process completely transparent and the base classifier receives only the reduced dataset. This is the full classname of the meta-classifier:

```text
 weka.classifiers.meta.AttributeSelectedClassifier
```
Example using `CfsSubsetEval` and `BestFirst`:

```bash
 java weka.classifiers.meta.AttributeSelectedClassifier \
   -t <training.arff> \
   -E "weka.attributeSelection.CfsSubsetEval -M" \
   -S "weka.attributeSelection.BestFirst -D 1 -N 5" \
   -W weka.classifiers.trees.J48 \
   -- \
   -C 0.25 -M 2
```

# Filter
In case you want to obtain the reduced/ranked data and not just output the selected/ranked attributes or using it internally in a classifier, you can use the filter approach. The following filter offers attribute selection:

```text
  weka.filters.supervised.attribute.AttributeSelection
```
Example using `CfsSubsetEval` and `BestFirst` in [batch mode](batch_filtering.md):

```bash
 java weka.filters.supervised.attribute.AttributeSelection \
    -E "weka.attributeSelection.CfsSubsetEval -M" \
    -S "weka.attributeSelection.BestFirst -D 1 -N 5" \
    -b \
    -i <input1.arff> \
    -o <output1.arff> \
    -r <input2.arff> \
    -s <output2.arff>
```
**Note:** batch mode is not available from the Explorer.

# See also
* [Batch filtering](batch_filtering.md) - general information about batch filtering
* [Use Weka in your Java code](use_weka_in_your_java_code.md), section [Attribute selection](use_weka_in_your_java_code.md#attribute-selection) - if you want to use attribute selection from your own code.
