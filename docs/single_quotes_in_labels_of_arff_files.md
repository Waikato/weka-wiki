Single quotes in Weka are used to surround strings with spaces or other special characters (see [spaces in labels of ARFF files](spaces_in_labels_of_arff_files.md)). If some of your labels contain single quotes, you have to escape them with a backslash.
For example, the name of one of Alexandre Duma's musketeers is
```text
D'Artagnan
```
and needs to be quoted and escaped as follows
```text
'D\'Artagnan'
```
Using the Weka API, you can use the `quote(String)` method of the `weka.core.Utils` class for doing this:

```java
import weka.core.Utils;
...
String raw = "D'Artagnan";
String escaped = Utils.quote(raw);
```