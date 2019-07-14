

The following examples show how to add weights to normal datasets and save them in the new XRFF data format. A version of Weka later than 3.5.3 (or the code from [Subversion](subversion.md)) is necessary for this code to work.

# Add arbitrary weights
```java
 import weka.core.converters.ConverterUtils.DataSource;
 import weka.core.converters.XRFFSaver;
 import weka.core.Instances;
 
 import java.io.File;
 
 /**
  * Loads file "args[0]", sets class if necessary (in that case the last 
  * attribute), adds some test weights and saves it as XRFF file
  * under "args[1]". E.g.: 
<br/>
  *   AddWeights anneal.arff anneal.xrff.gz
  *
  * @author FracPete (fracpete at waikato dot ac dot nz)
  */
 public class AddWeights {
   public static void main(String[] args) throws Exception {
     * load data
     DataSource source = new DataSource(args[0]);
     Instances data = source.getDataSet();
     if (data.classIndex() == -1)
       data.setClassIndex(data.numAttributes() - 1);
 
     * set weights
     double factor = 0.5  / (double) data.numInstances();
     for (int i = 0; i < data.numInstances(); i++) {
       data.instance(i).setWeight(0.5 + factor*i);
     }
 
     // save data
     XRFFSaver saver = new XRFFSaver();
     saver.setFile(new File(args[1]));
     saver.setInstances(data);
     saver.writeBatch();
   }
 }
```

# Add weights stored in an external file
```java
 import weka.core.converters.ConverterUtils.DataSource;
 import weka.core.converters.XRFFSaver;
 import weka.core.Instances;
 
 import java.io.BufferedReader;
 import java.io.File;
 import java.io.FileReader;
 
 /**
  * Loads file "args[0]" (can be ARFF, CSV, C4.5, etc.), sets class if necessary
  * (in that case the last attribute), adds weights from "args[1]" (one weight
  * per line) and saves it as XRFF file under "args[2]". E.g.: 
<br/>
  *   AddWeightsFromFile anneal.arff weights.txt anneal.xrff.gz
  *
  * @author FracPete (fracpete at waikato dot ac dot nz)
  */
 public class AddWeightsFromFile {
   public static void main(String[] args) throws Exception {
     * load data
     DataSource source = new DataSource(args[0]);
     Instances data = source.getDataSet();
     if (data.classIndex() == -1)
       data.setClassIndex(data.numAttributes() - 1);
 
     * read and set weights
     BufferedReader reader = new BufferedReader(new FileReader(args[1]));
     for (int i = 0; i < data.numInstances(); i++) {
       String line = reader.readLine();
       double weight = Double.parseDouble(line);
       data.instance(i).setWeight(weight);
     }
     reader.close();
 
     // save data
     XRFFSaver saver = new XRFFSaver();
     saver.setFile(new File(args[2]));
     saver.setInstances(data);
     saver.writeBatch();
   }
 }
```

# Add weights stored in the attribute
```java
 import weka.core.converters.ConverterUtils.DataSource;
 import weka.core.converters.XRFFSaver;
 import weka.core.Instances; 
 
 import java.io.File;
 
 /**
  * Loads file "args[0]", Adds weight given in attribute with
  * index "args[1]" - 1, deletes this attribute.
  * sets class if necessary (in that case the last 
  * attribute) and saves it as XRFF file
  * under "args[2]". E.g.: 
<br/>
  *   AddWeightsFromAtt file.arff 2 file.xrff.gz
  *
  * @author FracPete (fracpete at waikato dot ac dot nz)
  * @author gabi (gs23 at waikato dot ac dot nz)
  */
 public class AddWeightsFromAtt {
   public static void main(String[] args) throws Exception {
     * load data
     DataSource source = new DataSource(args[0]);
     Instances data = source.getDataSet(); 
 
     * get weight index
     int wIndex = Integer.parseInt(args[1]) - 1;
     
     * set weights
     for (int i = 0; i < data.numInstances(); i++) {
       double weight = data.instance(i).value(wIndex);
       data.instance(i).setWeight(weight);
     }
 
     * delete weight attribute and set class index
     data.deleteAttributeAt(wIndex);
     if (data.classIndex() == -1)
       data.setClassIndex(data.numAttributes() - 1);
 
     * save data
     XRFFSaver saver = new XRFFSaver();
     saver.setFile(new File(args[2]));
     saver.setInstances(data);
     saver.writeBatch();
   }
 }
```

# Download
* [AddWeights.java](files/AddWeights.java)
* [AddWeightsFromFile.java](files/AddWeightsFromFile.java)
* [AddWeightsFromAtt.java](files/AddWeightsFromAtt.java)

# See also
* [Subversion](subversion.md)
* The unofficial Weka package [dataset-weights](https://github.com/fracpete/dataset-weights-weka-package) allows you to modify attribute/instance weights using filters - no coding required