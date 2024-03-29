# DataSink
The easiest way to save an `weka.core.Instances` object to a file is by using
the [weka.core.converters.ConverterUtils.DataSink](https:/weka.sourceforge.io/doc.dev/weka/core/converters/ConverterUtils.DataSink.html) class.

```java
 import weka.core.converters.ConverterUtils.DataSink;
 import weka.core.Instances;

 Instances dataset = ...
 String outputFilename = ...
 try {
   DataSink.write(outputFilename, dataset);
 }
 catch (Exception e) {
   System.err.println("Failed to save data to: " + outputFilename);
   e.printStackTrace();
 }
```

# Converter
You can use the `ArffSaver` class (`weka.core.converters.ArffSaver`) for saving a `weka.core.Instances` object to a file.

Here is the snippet :

```java
 Instances dataSet = ...
 ArffSaver saver = new ArffSaver();
 saver.setInstances(dataSet);
 saver.setFile(new File("./data/test.arff"));
 saver.writeBatch();
```

**Notes:** 

* using the converter approach, one can easily swap the `ArffSaver` with another saver, e.g., the `CSVSaver` to output the data in a different format.
* The [Weka Examples](../weka_examples.md) collection dedicates quite a few examples to the use of converters in the `wekaexamples.core.converters` package ([stable](https://git.cms.waikato.ac.nz/weka/weka/-/tree/stable-3-8/wekaexamples/src/main/java/wekaexamples/core/converters/), [developer](https://git.cms.waikato.ac.nz/weka/weka/-/tree/main/trunk/wekaexamples/src/main/java/wekaexamples/core/converters/))

# Java I/O
You can also save the `weka.core.Instances` object directly using Java I/O classes:

```java
 import java.io.BufferedWriter;
 import java.io.FileWriter;
 ...
 Instances dataSet = ...
 BufferedWriter writer = new BufferedWriter(new FileWriter("./data/test.arff"));
 writer.write(dataSet.toString());
 writer.flush();
 writer.close();
```

**Note:** using the `toString()` of the `weka.core.Instances` doesn't scale very well for large datasets, since the complete string has to fit into memory. It is best to use a converter, as described in the previous section, which uses an incremental approach for writing the dataset to disk.

