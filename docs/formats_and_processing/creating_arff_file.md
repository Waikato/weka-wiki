The following code generates an `Instances` object and outputs it to stdout as [ARFF](arff.md) file.

It generates the following types of attributes:

* numeric
* nominal
* string
* date
* relational

Example class **AttTest**:

```java
import weka.core.Attribute;
import weka.core.DenseInstance;
import weka.core.Instances;

import java.util.ArrayList;

/**
 * Generates a little ARFF file with different attribute types.
 *
 * @author FracPete
 */
public class AttTest {
  public static void main(String[] args) throws Exception {
    ArrayList<Attribute> atts;
    ArrayList<Attribute> attsRel;
    ArrayList<String>    attVals;
    ArrayList<String>    attValsRel;
    Instances            data;
    Instances            dataRel;
    double[]             vals;
    double[]             valsRel;
    int                  i;

    // 1. set up attributes
    atts = new ArrayList<Attribute>();
    // - numeric
    atts.add(new Attribute("att1"));
    // - nominal
    attVals = new ArrayList<String>();
    for (i = 0; i < 5; i++)
      attVals.add("val" + (i+1));
    atts.add(new Attribute("att2", attVals));
    // - string
    atts.add(new Attribute("att3", (ArrayList<String>) null));
    // - date
    atts.add(new Attribute("att4", "yyyy-MM-dd"));
    // - relational
    attsRel = new ArrayList<Attribute>();
    // -- numeric
    attsRel.add(new Attribute("att5.1"));
    // -- nominal
    attValsRel = new ArrayList<String>();
    for (i = 0; i < 5; i++)
      attValsRel.add("val5." + (i+1));
    attsRel.add(new Attribute("att5.2", attValsRel));
    dataRel = new Instances("att5", attsRel, 0);
    atts.add(new Attribute("att5", dataRel, 0));

    // 2. create Instances object
    data = new Instances("MyRelation", atts, 0);

    // 3. fill with data
    // first instance
    vals = new double[data.numAttributes()];
    // - numeric
    vals[0] = Math.PI;
    // - nominal
    vals[1] = attVals.indexOf("val3");
    // - string
    vals[2] = data.attribute(2).addStringValue("This is a string!");
    // - date
    vals[3] = data.attribute(3).parseDate("2001-11-09");
    // - relational
    dataRel = new Instances(data.attribute(4).relation(), 0);
    // -- first instance
    valsRel = new double[2];
    valsRel[0] = Math.PI + 1;
    valsRel[1] = attValsRel.indexOf("val5.3");
    dataRel.add(new DenseInstance(1.0, valsRel));
    // -- second instance
    valsRel = new double[2];
    valsRel[0] = Math.PI + 2;
    valsRel[1] = attValsRel.indexOf("val5.2");
    dataRel.add(new DenseInstance(1.0, valsRel));
    vals[4] = data.attribute(4).addRelation(dataRel);
    // add
    data.add(new DenseInstance(1.0, vals));

    // second instance
    vals = new double[data.numAttributes()];  // important: needs NEW array!
    // - numeric
    vals[0] = Math.E;
    // - nominal
    vals[1] = attVals.indexOf("val1");
    // - string
    vals[2] = data.attribute(2).addStringValue("And another one!");
    // - date
    vals[3] = data.attribute(3).parseDate("2000-12-01");
    // - relational
    dataRel = new Instances(data.attribute(4).relation(), 0);
    // -- first instance
    valsRel = new double[2];
    valsRel[0] = Math.E + 1;
    valsRel[1] = attValsRel.indexOf("val5.4");
    dataRel.add(new DenseInstance(1.0, valsRel));
    // -- second instance
    valsRel = new double[2];
    valsRel[0] = Math.E + 2;
    valsRel[1] = attValsRel.indexOf("val5.1");
    dataRel.add(new DenseInstance(1.0, valsRel));
    vals[4] = data.attribute(4).addRelation(dataRel);
    // add
    data.add(new DenseInstance(1.0, vals));

    // 4. output data
    System.out.println(data);
  }
}
```

# Missing values
By default, a new double array will be initialized with 0s. In case you want to be a value missing at a certain position, you have to explicitly set the *missing value* via the `missingValue()` method of the `weka.core.Utils` class. In case you already have an existing `weka.core.Instance` object, then you use its `setMissing(int)` method, which sets a missing value at the given position. Here are examples, which set the **third** attribute to missing:

* double array:

    ```
    double[] vals = ...  // from somewhere, e.g., from AttTest.java example
    vals[2] = Utils.missingValue();
    ```

* `weka.core.Instance` object:

    ```
    double[] vals = ... // from somewhere, e.g., from AttTest.java example
    Instance inst = new DenseInstance(1.0, vals);
    inst.setMissing(2);
    ```

# Downloads
* [AttTest.java](../files/AttTest.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/core/CreateInstances.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/core/CreateInstances.java)) - the above class

# See also
* [Save Instances to an ARFF File](save_instances_to_arff.md) - if you want to save the data to a file instead of printing it to stdout
* [Adding attributes to a dataset](../adding_attributes_to_dataset.md) - shows how to add attributes to an existing dataset
* [ARFF](arff.md) format
