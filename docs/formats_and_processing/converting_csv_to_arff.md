
For converting CSV (comma separated value) files into [ARFF](arff.md) files you need the following two [converters](https:/weka.sourceforge.io/doc.dev/weka/core/converters/package-frame.html):

- [CSVLoader](https:/weka.sourceforge.io/doc.dev/weka/core/converters/CSVLoader.html) for loading the CSV file into an [Instances](https:/weka.sourceforge.io/doc.dev/weka/core/Instances.html) object
- [ArffSaver](https:/weka.sourceforge.io/doc.dev/weka/core/converters/ArffSaver.html) to save the [Instances](https:/weka.sourceforge.io/doc.dev/weka/core/Instances.html) as an ARFF file

In the following you'll find some example code to show you how to use the [converters](https:/weka.sourceforge.io/doc.dev/weka/core/converters/package-frame.html). The class takes 2 arguments:

- the *input* CSV file
- the *output* [ARFF](arff.md) file

Example code:
```java
import weka.core.Instances;
import weka.core.converters.ArffSaver;
import weka.core.converters.CSVLoader;

import java.io.File;

public class CSV2Arff {
  /**
   * takes 2 arguments:
   * - CSV input file
   * - ARFF output file
   */
  public static void main(String[] args) throws Exception {
    if (args.length != 2) {
      System.out.println("\nUsage: CSV2Arff <input.csv> <output.arff>\n");
      System.exit(1);
    }
    
    // load CSV
    CSVLoader loader = new CSVLoader();
    loader.setSource(new File(args[0]));
    Instances data = loader.getDataSet();

    // save ARFF
    ArffSaver saver = new ArffSaver();
    saver.setInstances(data);
    saver.setFile(new File(args[1]));
    saver.setDestination(new File(args[1]));
    saver.writeBatch();
  }
}
```
**Note:** with versions of Weka later than 3.5.3 the call of `saver.setDestination(new File(args[1]));` is no longer necessary, it is automatically done in the `saver.setFile(new File(args[1]));` method.

# See also 
The [Weka Examples](../weka_examples) collection dedicates several example classes of loading from and saving to various file formats:

* [book](https://svn.cms.waikato.ac.nz/svn/weka/branches/book2ndEd-branch/wekaexamples/src/main/java/wekaexamples/core/converters/)
* [stable-3-6](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-6/wekaexamples/src/main/java/wekaexamples/core/converters/)
* [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/core/converters/)