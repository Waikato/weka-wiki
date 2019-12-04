
You should use the BIFReader class (`weka.classifiers.bayes.net.BIFReader`).

Here is the snippet :

```java
 import weka.classifiers.bayes.BayesNet;
 import weka.classifiers.bayes.net.BIFReader;
 
 public class wekaTest {
  public static void main(String[] args) throws Exception {
   BayesNet network = new BayesNet();
   BIFReader reader = new BIFReader();	
   network = reader.processFile("rb_on_min_attr.xml");
  }
 }
```

https:/weka.sourceforge.io/doc.dev/weka/classifiers/evaluation/ThresholdCurve.html

# Downloads
* `LoadBif.java` ([book](https://svn.cms.waikato.ac.nz/svn/weka/branches/book2ndEd-branch/wekaexamples/src/main/java/wekaexamples/classifiers/LoadBIF.java), [stable-3.8](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/classifiers/LoadBIF.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/classifiers/LoadBIF.java))