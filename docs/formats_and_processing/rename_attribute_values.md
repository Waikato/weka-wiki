
After discretizing an attribute you might want to rename the values of the newly created nominal attribute. E.g., after discretizing a numeric attribute with values ranging from 1 to 100 you might end up with a nominal attribute that has the following values:

```text
 '1-15','16-18','29-100'
```
You can use the `renameAttributeValue(...)` method of the `weka.core.Instances` class (see [API](http://weka.sourceforge.net/doc.stable/weka/core/Instances.html)) to rename this values into, e.g., 0, 1 and 2. Here's a code snippet how to do this (`arff` is an Instances object, `att` is an attribute of the same instances object):

```java
 for (int n = 0; n < att.numValues(); n++) {
    arff.renameAttributeValue(att, att.value(n), "" + n);
```
The [Rename.java](../files/Rename.java) like mentioned above.

# See also
* [Use Weka in your Java code](../use_weka_in_your_java_code.md)

# Downloads
* [Rename.java](../files/Rename.java)
