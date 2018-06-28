**Note:** This is also covered in chapter *Extending WEKA* of the WEKA manual.

# Packages
A few comments about the different filter sub-packages:

* `supervised` - contains supervised filters, i.e., filters that take class distributions into account. Must implement the `weka.filters.SupervisedFilter` interface.

    * `attribute` - filters that work column-wise.
    * `instance` - filters that work row-wise.

* `unsupervised` - contains unsupervised filters, i.e., they work without taking any class distributions into account. Must implement the `weka.filters.UnsupervisedFilter` interface.

    * `attribute` - filters that work column-wise.
    * `instance` - filters that work row-wise.

# Choosing the superclass
The base filters and interfaces are all located in the following package:

```
 weka.filters
```

One can basically distinguish between two different kinds of filters:

* **batch filters** - they need to see the whole dataset before they can start processing it, which they do in one go
* **stream filters** - they can start producing output right away and the data just passes through while being modified

All filters are derived from the abstract superclass `weka.filters.Filter`.

To speed up development, there are also the following abstract filters, depending on the kind of classifier you want to implement:

* `weka.filters.SimpleBatchFilter`
* `weka.filters.SimpleStreamFilter`

These filters simplify the rather general and complex framework introduced by the abstract superclass `weka.filters.Filter`. One only needs to implement a couple of abstract methods that will process the actual data and override, if necessary, a few existing methods for option handling.

## Filter

### Implementation
The following methods are of importance for the implementation of a filter and explained in detail further down:

* `getCapabilities()`
* `setInputFormat(Instances)`
* `getInputFormat()`
* `setOutputFormat(Instances)`
* `getOutputFormat()`
* `input(Instance)`
* `bufferInput(Instance)`
* `push(Instance)`
* `output()`
* `batchFinished()`
* `flushInput()`
* `getRevision()`

But only the following ones need normally be modified:

* `getCapabilities()`
* `setInputFormat(Instances)`
* `input(Instance)`
* `batchFinished()`
* `getRevision()`

#### getCapabilities()
Filters implement the `weka.core.CapabilitiesHandler` interface like the classifiers. This method returns what kind of data the filter is able to process. Needs to be adapted for each individual filter.

#### setInputFormat(Instances)
With this format, the user tells the filter what format, i.e., attributes, the input data has. This method also tests, whether the filter can actually process this data. All older Weka versions or book branch versions need to check the data manually and throw fitting exceptions, e.g., not being able to handle String attributes.

If the output format of the filter, i.e., the new Instances header, can be determined based alone on this information, then the method should set the output format via `setOutputFormat(Instances)` and return `true`, otherwise it has to return `false`.

#### getInputFormat()
This method returns an Instances object containing all currently buffered Instance objects from the input queue.

#### setOutputFormat(Instances)
`setOutputFormat(Instances)` defines the new Instances header for the output data. For filters that work on a row-basis, there shouldn't be any changes between the input and output format. But filters that work on attributes, e.g., removing, adding, modifying, will affect this format. This method must be called with the appropriate Instances object as parameter, since all Instance objects being processed will rely on the output format.

#### getOutputFormat()
This method returns the currently set Instances object that defines the output format. In case `setOutputFormat(Instances)` hasn't been called yet, this method will return `null`.

#### input(Instance)
The `input(Instance)` method returns `true` if the given Instance can be processed straight away and can be collected immediately via the `output()` method (after adding it to the output queue via `push(Instance)`, of course). This is also the case if the first batch of data has been processed and the instance belongs to the second batch. Via `isFirstBatchDone()` one can query whether this instance is still part of the first batch or of the second.

If the Instance cannot be processed immediately, e.g., the filter needs to collect all the data first before doing some calculations, then it needs to be buffered with `bufferInput(Instance)` until `batchFinished()` is called.

#### bufferInput(Instance)
In case an Instance cannot be processed immediately, one can use this method to buffer them in the input queue. All buffered Instance objects are available via the `getInputFormat()` method.

#### push(Instance)
`push(Instance)` adds the given Instance to the output queue.

#### output()
Returns the next Instance object from the output queue and removes it from there. In case there is no Instance available this method returns `null`.

#### batchFinished()
The `batchFinished()` method signifies the end of a dataset being pushed through the filter. In case of a filter that couldn't process the data of the first batch immediately, this is the place to determine what the output format will be (and set if via `setOutputFormat(Instances)`) and process the actual data. The currently available data can be retrieved with the `getInputFormat()` method. After processing the data, one needs to call `flushInput()` to remove all the pending input data.

#### flushInput()
`flushInput()` removes all buffered Instance objects from the input queue. This method must be called after all the Instance objects have been processed in the `batchFinished()` method.

### Option handling
If the filter should be able to handle commandline options, then the weka.core.OptionHandler interface needs to be implemented. In addition to that, the following code should be added at the end of the `setOptions(String[])` method:

```java
 if (getInputFormat() != null)
    setInputFormat(getInputFormat());
```

This will inform the filter about changes in the options and therefore reset it.

### Examples
The following examples are to illustrate the filter framework. 

**Note:** unseeded random number generators like `Math.random()` should never be used since they will produce different results in each run and repeatable results are essential in machine learning.

#### BatchFilter
This simple batch filter adds a new attribute called //bla// at the end of the dataset. The rows of this attribute contain only the row's index in the data. Since the batch-filter need not see all the data before creating the output format, the `setInputFormat(Instances)` sets the output format and returns `true` (indicating that the output format can be queried immediately). The `batchFinished()` method performs the processing of all the data.

```java
  import weka.core.*;
  import weka.core.Capabilities.*;
 
  public class BatchFilter
    extends Filter {
    
    public String globalInfo() {
      return   "A batch filter that adds an additional attribute 'bla' at the end "
             + "containing the index of the processed instance. The output format "
             + "can be collected immediately.";
    }
       
    public Capabilities getCapabilities() {
      Capabilities result = super.getCapabilities();
      result.enableAllAttributes();
      result.enableAllClasses();
      result.enable(Capability.NO_CLASS);  // filter doesn't need class to be set
      return result;
    }
 
    public boolean setInputFormat(Instances instanceInfo) throws Exception {
      super.setInputFormat(instanceInfo);
 
      Instances outFormat = new Instances(instanceInfo, 0);
      outFormat.insertAttributeAt(new Attribute("bla"), outFormat.numAttributes());
      setOutputFormat(outFormat);
 
      return true;  // output format is immediately available
    }
 
    public boolean batchFinished() throws Exception {
      if (getInputFormat() = null)
        throw new NullPointerException("No input instance format defined");
 
      Instances inst = getInputFormat();
      Instances outFormat = getOutputFormat();
      for (int i = 0; i < inst.numInstances(); i++) {
        double[] newValues = new double[outFormat.numAttributes()];
        double[] oldValues = inst.instance(i).toDoubleArray();
        System.arraycopy(oldValues, 0, newValues, 0, oldValues.length);
        newValues[newValues.length - 1] = i;
        push(new Instance(1.0, newValues));
      }
      
      flushInput();
      m_NewBatch = true;
      m_FirstBatchDone = true;
      return (numPendingOutput() != 0);
    }
 
    public static void main(String[] args) {
      runFilter(new BatchFilter(), args);
    }
  }
```

#### BatchFilter2
In contrast to the first batch filter, this one here cannot determine the output format immediately (the number of instances in the first batch is part of the attribute name now). This is done in the `batchFinished()` method.

```java
  import weka.core.*;
  import weka.core.Capabilities.*;
  
  public class BatchFilter2
    extends Filter {
    
    public String globalInfo() {
      return   "A batch filter that adds an additional attribute 'bla' at the end "
             + "containing the index of the processed instance. The output format "
             + "cannot be collected immediately.";
    }
       
    public Capabilities getCapabilities() {
      Capabilities result = super.getCapabilities();
      result.enableAllAttributes();
      result.enableAllClasses();
      result.enable(Capability.NO_CLASS);  // filter doesn't need class to be set
      return result;
    }
 
    public boolean batchFinished() throws Exception {
      if (getInputFormat() = null)
        throw new NullPointerException("No input instance format defined");

      // output format still needs to be set (depends on first batch of data)
      if (!isFirstBatchDone()) {
        Instances outFormat = new Instances(getInputFormat(), 0);
        outFormat.insertAttributeAt(new Attribute(
            "bla-" + getInputFormat().numInstances()), outFormat.numAttributes());
        setOutputFormat(outFormat);
      }
      
      Instances inst = getInputFormat();
      Instances outFormat = getOutputFormat();
      for (int i = 0; i < inst.numInstances(); i++) {
        double[] newValues = new double[outFormat.numAttributes()];
        double[] oldValues = inst.instance(i).toDoubleArray();
        System.arraycopy(oldValues, 0, newValues, 0, oldValues.length);
        newValues[newValues.length - 1] = i;
        push(new Instance(1.0, newValues));
      }
      
      flushInput();
      m_NewBatch = true;
      m_FirstBatchDone = true;
      
      return (numPendingOutput() != 0);
    }
 
    public static void main(String[] args) {
      runFilter(new BatchFilter2(), args);
    }
  }
```

#### BatchFilter3
As soon as this batch filter's first batch is done, it can process Instance objects immediately in the `input(Instance)` method. It adds a new attribute which contains just a random number, but the random number generator being used is seeded with the number of instances from the first batch.

```java
  import weka.core.*;
  import weka.core.Capabilities.*;
   
  import java.util.Random;
   
  public class BatchFilter3
    extends Filter {
   
    protected int m_Seed;
    protected Random m_Random;
    
    public String globalInfo() {
      return   "A batch filter that adds an attribute 'bla' at the end "
             + "containing a random number. The output format cannot be collected "
             + "immediately.";
    }
       
    public Capabilities getCapabilities() {
      Capabilities result = super.getCapabilities();
      result.enableAllAttributes();
      result.enableAllClasses();
      result.enable(Capability.NO_CLASS);  // filter doesn't need class to be set
      return result;
    }
   
    public boolean input(Instance instance) throws Exception {
      if (getInputFormat() = null)
        throw new NullPointerException("No input instance format defined");
   
      if (isNewBatch()) {
        resetQueue();
        m_NewBatch = false;
      }
   
      if (isFirstBatchDone())
        convertInstance(instance);
      else
        bufferInput(instance);
      
      return isFirstBatchDone();
    }
   
    public boolean batchFinished() throws Exception {
      if (getInputFormat() = null)
        throw new NullPointerException("No input instance format defined");
   
      // output format still needs to be set (random number generator is seeded
      // with number of instances of first batch)
      if (!isFirstBatchDone()) {
        m_Seed = getInputFormat().numInstances();
        Instances outFormat = new Instances(getInputFormat(), 0);
        outFormat.insertAttributeAt(new Attribute(
            "bla-" + getInputFormat().numInstances()), outFormat.numAttributes());
        setOutputFormat(outFormat);
      }
   
      Instances inst = getInputFormat();
      for (int i = 0; i < inst.numInstances(); i++) {
        convertInstance(inst.instance(i));
      }
      
      flushInput();
      m_NewBatch = true;
      m_FirstBatchDone = true;
      m_Random = null;
      
      return (numPendingOutput() != 0);
    }
   
    protected void convertInstance(Instance instance) {
      if (m_Random = null)
        m_Random = new Random(m_Seed);
      double[] newValues = new double[instance.numAttributes() + 1];
      double[] oldValues = instance.toDoubleArray();
      newValues[newValues.length - 1] = m_Random.nextInt();
      System.arraycopy(oldValues, 0, newValues, 0, oldValues.length);
      push(new Instance(1.0, newValues));
    }
    
    public static void main(String[] args) {
      runFilter(new BatchFilter3(), args);
    }
  }
```

#### StreamFilter
This stream filter adds a random number at the end of each instance of the input data. Since this doesn't rely on having access to the full data of the first batch, the output format is accessible immediately after using `setInputFormat(Instances)`. All the Instance objects are immediately processed in `input(Instance)` via the `convertInstance(Instance)` method, which pushes them immediately to the output queue.

```java
  import weka.core.*;
  import weka.core.Capabilities.*;
 
  import java.util.Random;
 
  public class StreamFilter
    extends Filter {
 
    protected Random m_Random;
    
    public String globalInfo() {
      return   "A stream filter that adds an attribute 'bla' at the end "
             + "containing a random number. The output format can be collected "
             + "immediately.";
    }
       
    public Capabilities getCapabilities() {
      Capabilities result = super.getCapabilities();
      result.enableAllAttributes();
      result.enableAllClasses();
      result.enable(Capability.NO_CLASS);  // filter doesn't need class to be set
      return result;
    }
 
    public boolean setInputFormat(Instances instanceInfo) throws Exception {
      super.setInputFormat(instanceInfo);
 
      Instances outFormat = new Instances(instanceInfo, 0);
      outFormat.insertAttributeAt(new Attribute("bla"), outFormat.numAttributes());
      setOutputFormat(outFormat);
 
      m_Random = new Random(1);
 
      return true;  // output format is immediately available
    }
 
    public boolean input(Instance instance) throws Exception {
      if (getInputFormat() = null)
        throw new NullPointerException("No input instance format defined");
 
      if (isNewBatch()) {
        resetQueue();
        m_NewBatch = false;
      }
      
      convertInstance(instance);
      
      return true;  // can be immediately collected via output()
    }
 
    protected void convertInstance(Instance instance) {
      double[] newValues = new double[instance.numAttributes() + 1];
      double[] oldValues = instance.toDoubleArray();
      newValues[newValues.length - 1] = m_Random.nextInt();
      System.arraycopy(oldValues, 0, newValues, 0, oldValues.length);
      push(new Instance(1.0, newValues));
    }
    
    public static void main(String[] args) {
      runFilter(new StreamFilter(), args);
    }
  }
```

## SimpleBatchFilter
Only the following abstract methods need to be implemented:

* `globalInfo()` - returns a short description of what the filter does; will be displayed in the GUI
* `determineOutputFormat(Instances)` - generates the new format, based on the input data
* `process(Instances)` - processes the whole dataset in one go
* `getRevision()` - returns the [Subversion](subversion.md) revision information

If you need access to the full input dataset in `determineOutputFormat(Instances)`, then you need to also override the method `allowAccessToFullInputFormat()` and make it return true. 

If more options are necessary, then the following methods need to be overridden:

* `listOptions()` - returns an enumeration of the available options; these are printed if one calls the filter with the *-h* option
* `setOptions(String[])` - parses the given option array, that were passed from commandline
* `getOptions()` - returns an array of options, resembling the current setup of the filter

In the following an **example implementation** that adds an additional attribute at the end, containing the index of the processed instance:

```java
 import weka.core.*;
 import weka.core.Capabilities.*;
 import weka.filters.*;

 public class SimpleBatch
   extends SimpleBatchFilter {

   public String globalInfo() {
     return   "A simple batch filter that adds an additional attribute 'bla' at the end "
            + "containing the index of the processed instance.";
   }

   public Capabilities getCapabilities() {
     Capabilities result = super.getCapabilities();
     result.enableAllAttributes();
     result.enableAllClasses();
     result.enable(Capability.NO_CLASS);  //// filter doesn't need class to be set//
     return result;
   }

   protected Instances determineOutputFormat(Instances inputFormat) {
     Instances result = new Instances(inputFormat, 0);
     result.insertAttributeAt(new Attribute("bla"), result.numAttributes());
     return result;
   }

   protected Instances process(Instances inst) {
     Instances result = new Instances(determineOutputFormat(inst), 0);
     for (int i = 0; i < inst.numInstances(); i++) {
       double[] values = new double[result.numAttributes()];
       for (int n = 0; n < inst.numAttributes(); n++)
         values[n] = inst.instance(i).value(n);
       values[values.length - 1] = i;
       result.add(new Instance(1, values));
     }
     return result;
   }

   public static void main(String[] args) {
     runFilter(new SimpleBatch(), args);
   }
 }
```

## SimpleStreamFilter
Only the following abstract methods need to be implemented:

* `globalInfo()` - returns a short description of what the filter does; will be displayed in the GUI
* `determineOutputFormat(Instances)` - generates the new format, based on the input data
* `process(Instance)`processes a single instance and turns it from the old format into the new one
* `getRevision()` - returns the [Subversion](subversion.md) revision information

The `reset()` method is only used, since the random number generator needs to be re-initialized in order to obtain repeatable results.

If more options are necessary, then the following methods need to be overridden:

* `listOptions()` - returns an enumeration of the available options; these are printed if one calls the filter with the *-h* option
* `setOptions(String[])` - parses the given option array, that were passed from commandline
* `getOptions()` - returns an array of options, resembling the current setup of the filter

In the following an **example implementation** of a stream filter that adds an extra attribute at the end, which is filled with random numbers:

```java
 import weka.core.*;
 import weka.core.Capabilities.*;
 import weka.filters.*;

 import java.util.Random;

 public class SimpleStream
   extends SimpleStreamFilter {

   protected Random m_Random;

   public String globalInfo() {
     return   "A simple stream filter that adds an attribute 'bla' at the end "
            + "containing a random number.";
   }

   public Capabilities getCapabilities() {
     Capabilities result = super.getCapabilities();
     result.enableAllAttributes();
     result.enableAllClasses();
     result.enable(Capability.NO_CLASS);  //// filter doesn't need class to be set//
     return result;
   }

   protected void reset() {
     super.reset();
     m_Random = new Random(1);
   }

   protected Instances determineOutputFormat(Instances inputFormat) {
     Instances result = new Instances(inputFormat, 0);
     result.insertAttributeAt(new Attribute("bla"), result.numAttributes());
     return result;
   }

   protected Instance process(Instance inst) {
     double[] values = new double[inst.numAttributes() + 1];
     for (int n = 0; n < inst.numAttributes(); n++)
       values[n] = inst.value(n);
     values[values.length - 1] = m_Random.nextInt();
     Instance result = new Instance(1, values);
     return result;
   }

   public static void main(String[] args) {
     runFilter(new SimpleStream(), args);
   }
 }
```

A **real-world** implementation of a stream filter is the `MultiFilter` (package `weka.filters`), which passes the data through all the filters it contains. Depending on whether all the used filters are streamable or not, it acts either as a stream filter or as batch filter.

# Internals
Some useful methods of the filter classes:

* `isNewBatch()` - returns `true` if an instance of the filter was just instantiated via `new` or a new batch was started via the `batchFinished()` method.
* `isFirstBatchDone()` - returns `true` as soon as the first batch was finished via the `batchFinished()` method. Useful for *supervised* filters, which should not be altered after being trained with the first batch of instances.

# Revisions
Filters also implement the `weka.core.RevisionHandler` interface. This provides the functionality of obtaining the [Subversion](subversion.md) revision from within Java. Filters that are not part of the official Weka distribution will have to implement the method `getRevision()` as follows, which will return a dummy revision of *1.0*:

```java
  /**
   * Returns the revision string.
   * 
   * @return		the revision
   */
  public String getRevision() {
    return RevisionUtils.extract("$Revision: 1.0 $");
  }
```

# Integration
After finishing the coding stage, it's time to integrate your filter in the Weka framework, i.e., to make it available in the Explorer, Experimenter, etc.

The [[GenericObjectEditor]] article shows you how to tell Weka where to find your filter and therefore displaying it in the *GenericObjectEditor* (filters work in the same fashion as classifiers, regarding the discovery).

# Testing
Weka provides already a test framework to ensure the basic functionality of a filter. It is essential for the filter to pass these tests.

## Option handling 
If your filter implements `weka.core.OptionHandler`, check the **option handling** of your filter with the following tool from commandline:

```
 weka.core.CheckOptionHandler -W classname [-- additional parameters]
```

All tests need to return *yes*.

## GenericObjectEditor
The `CheckGOE` class checks whether all the properties available in the GUI have a tooltip accompanying them and whether the `globalInfo()` method is declared:

```
 weka.core.CheckGOE -W classname [-- additional parameters]
```

All tests, once again, need to return *yes*.

## Source code
Filters that implement the `weka.filters.Sourcable` interface can output Java code of their internal representation. In order to check the generated code, one should not only compile the code, but also test it with the following test class:

```
 weka.filters.CheckSource
```

This class takes the original Weka filter, the generated code and the dataset used for generating the source code (and an optional class index) as parameters. It builds the Weka filter on the dataset and compares the output, the one from the Weka filter and the one from the generated source code, whether they are the same.

Here's an example call for `weka.filters.unsupervised.attribute.ReplaceMissingValues` and the generated class `weka.filters.WekaWrapper` (it wraps the actual generated code in a pseudo-filter):

```
 java weka.filters.CheckSource \
    -W weka.filters.unsupervised.attribute.ReplaceMissingValues \
    -S weka.filters.WekaWrapper \
    -t data.arff
```

It needs to return *Tests OK!*.

## Unit tests
In order to make sure that your filter applies to the Weka criteria, you should add your filter to the [junit](http://www.junit.org/) unit test framework, i.e., by creating a Test class.

How to check out the unit test framework, you can find [here](subversion.md#junit).


# See also
* [[GenericObjectEditor|GenericObjectEditor/GenericPropertiesCreator]]

# Links 
* [junit](http://www.junit.org/)

