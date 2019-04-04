Yes, you can. But be aware that there is a drawback in comparison to [ARFF](../arff.md) files (WEKA's default file format):

**Train and test set may not be compatible.** Using CSV files as train and test set can be a frustrating exercise. Since CSV files don't contain any information about the attributes, WEKA needs to determine the labels for nominal attributes itself. Not only does the order of the appearance of these labels create different nominal attributes ("1,2,3" vs "1,3,2"), but it is also not guaranteed that all the labels that appeared in the train set also appear in the test set ("1,2,3,4" vs "1,3,4") and vice versa.

