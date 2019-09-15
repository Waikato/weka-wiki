A common problem people have with [ARFF](arff.md) files is that labels can only have spaces if they are enclosed in single quotes, i.e., a label such as:

```text
 some value
```
should be written either
```text
 'some value'
```
or
```text
 some_value
```
in the file.

See [single quotes in labels of ARFF files](single_quotes_in_labels_of_arff_files.md) for an example using the Weka API for automatically quoting such strings.