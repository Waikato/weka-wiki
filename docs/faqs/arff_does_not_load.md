One way to figure out why [ARFF](../arff.md) files are failing to load is to give them to the `weka.core.Instances` class. In the SimpleCLI or in the terminal, type the following:

```
 java weka.core.Instances filename.arff
```

where you substitute `filename` for the actual name of your file. This should return an error if there is a problem reading the file, or show some statistics if the file is ok. The error message you get should give some indication of what is wrong.

# nominal value not declared in header, read Token[X], line Y
If you get this error message than you seem to have declared a nominal attribute in the ARFF header section, but WEKA came across a value (**"X"**) in the data (in line **Y**) for this particular attribute that wasn't listed as possible value.

**All** nominal values that appear in the data must be declared in the header.
