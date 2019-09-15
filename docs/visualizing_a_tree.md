
The following code sample (`VisualizeJ48.java`) takes an [ARFF](formats_and_processing/arff.md) file as input, trains a `[J48](http://weka.sourceforge.net/doc/weka/classifiers/trees/j48.html)` and displays the generated tree with the `[TreeVisualizer](http://weka.sourceforge.net/doc/weka/gui/treevisualizer/treevisualizer.html)` class.

This can be done with all classifiers that implement the `[weka.core.Drawable](http://weka.sourceforge.net/doc/weka/core/drawable.html)` interface.

# Source code
```java
import java.awt.BorderLayout;
 import java.awt.event.WindowAdapter;
 import java.awt.event.WindowEvent;
 import java.io.BufferedReader;
 import java.io.FileReader;
 
 import javax.swing.JFrame;
 
 import weka.classifiers.trees.J48;
 import weka.core.Instances;
 import weka.gui.treevisualizer.PlaceNode2;
 import weka.gui.treevisualizer.TreeVisualizer;
 
 /**
  * Displays a trained J48 as tree.
  * Expects an ARFF filename as first argument.
  *
  * @author FracPete (fracpete at waikato dot ac dot nz)
  */
 public class VisualizeJ48 {
   public static void main(String args[]) throws Exception {
     // train classifier
     J48 cls = new J48();
     Instances data = new Instances(new BufferedReader(new FileReader(args[0])));
     data.setClassIndex(data.numAttributes() - 1);
     cls.buildClassifier(data);
 
     // display classifier
     final javax.swing.JFrame jf = 
       new javax.swing.JFrame("Weka Classifier Tree Visualizer: J48");
     jf.setSize(500,400);
     jf.getContentPane().setLayout(new BorderLayout());
     TreeVisualizer tv = new TreeVisualizer(null,
         cls.graph(),
         new PlaceNode2());
     jf.getContentPane().add(tv, BorderLayout.CENTER);
     jf.addWindowListener(new java.awt.event.WindowAdapter() {
       public void windowClosing(java.awt.event.WindowEvent e) {
         jf.dispose();
       }
     });
 
     jf.setVisible(true);
     tv.fitToScreen();
   }
 }
```

# Downloads
* [VisualizeJ48.java](files/VisualizeJ48.java)
