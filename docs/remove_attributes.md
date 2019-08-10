
The following code removes specified attributes from an [ARFF](arff.md) file and prints the result to stdout.

The class takes the following parameters:

* [ARFF](arff.md) file
* attribute index/indices
* inversion of index/indices (false/true)
```java
 import weka.core.Instances;
 import weka.filters.Filter;
 import weka.filters.unsupervised.attribute.Remove;
 
 import java.io.BufferedReader;
 import java.io.FileReader;
 
 public class RemoveTest {
   /**
    * takes an ARFF file as first argument, the number of indices to remove
    * as second and thirdly whether to invert or not (true/false).
    * Dumps the generated data to stdout.
    */
   public static void main(String[] args) throws Exception {
     Instances       inst;
     Instances       instNew;
     Remove          remove;
 
     inst   = new Instances(new BufferedReader(new FileReader(args[0])));
     remove = new Remove();
     remove.setAttributeIndices(args[1]);
     remove.setInvertSelection(new Boolean(args[2]).booleanValue());
     remove.setInputFormat(inst);
     instNew = Filter.useFilter(inst, remove);
     System.out.println(instNew);
   }
 }
```
The same can be achieved with the following filter commandline (add `-V` to invert the selection):

```bash
 java weka.filters.unsupervised.attribute.Remove -R <indices> -i <input> -o <output>
```

# Downloads
* `RemoveTest.java` ([book](https://svn.cms.waikato.ac.nz/svn/weka/branches/book2ndEd-branch/wekaexamples/src/main/java/wekaexamples/filters/RemoveTest.java), [stable-3.6](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-6/wekaexamples/src/main/java/wekaexamples/filters/RemoveTest.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/filters/RemoveTest.java))