The following little Java class trains a NaiveBayes classifier with a dataset provided by the user and displays the ROC curve for the first class label.

Source code:

```java
 import java.awt.*;
 import java.io.*;
 import java.util.*;
 import javax.swing.*;
 import weka.core.*;
 import weka.classifiers.*;
 import weka.classifiers.bayes.NaiveBayes;
 import weka.classifiers.evaluation.Evaluation;
 import weka.classifiers.evaluation.ThresholdCurve;
 import weka.gui.visualize.*;

 /**
   * Generates and displays a ROC curve from a dataset. Uses a default
   * NaiveBayes to generate the ROC data.
   *
   * @author FracPete
   */
 public class GenerateROC {

   /**
    * takes one argument: dataset in ARFF format (expects class to
    * be last attribute)
    */
   public static void main(String[] args) throws Exception {
     // load data
     Instances data = new Instances(
                           new BufferedReader(
                             new FileReader(args[0])));
     data.setClassIndex(data.numAttributes() - 1);

     // train classifier
     Classifier cl = new NaiveBayes();
     Evaluation eval = new Evaluation(data);
     eval.crossValidateModel(cl, data, 10, new Random(1));

     // generate curve
     ThresholdCurve tc = new ThresholdCurve();
     int classIndex = 0;
     Instances result = tc.getCurve(eval.predictions(), classIndex);

     // plot curve
     ThresholdVisualizePanel vmc = new ThresholdVisualizePanel();
     vmc.setROCString("(Area under ROC = " +
         Utils.doubleToString(tc.getROCArea(result), 4) + ")");
     vmc.setName(result.relationName());
     PlotData2D tempd = new PlotData2D(result);
     tempd.setPlotName(result.relationName());
     tempd.addInstanceNumberAttribute();
     // specify which points are connected
     boolean[] cp = new boolean[result.numInstances()];
     for (int n = 1; n < cp.length; n++)
       cp[n] = true;
     tempd.setConnectPoints(cp);
     // add plot
     vmc.addPlot(tempd);

     // display curve
     String plotName = vmc.getName();
     final javax.swing.JFrame jf =
       new javax.swing.JFrame("Weka Classifier Visualize: "+plotName);
     jf.setSize(500,400);
     jf.getContentPane().setLayout(new BorderLayout());
     jf.getContentPane().add(vmc, BorderLayout.CENTER);
     jf.addWindowListener(new java.awt.event.WindowAdapter() {
       public void windowClosing(java.awt.event.WindowEvent e) {
       jf.dispose();
       }
     });
     jf.setVisible(true);
   }
 }
```

# See also
* [ROC curves](roc_curves.md)
* [Visualizing ROC curve](visualization/visualizing_roc_curve.md)
* [Plotting multiple ROC curves](plotting_multiple_roc_curves.md)

# Downloads
* [GenerateROC.java](files/GenerateROC.java) ([stable](https://git.cms.waikato.ac.nz/weka/weka/-/tree/stable-3-8/wekaexamples/src/main/java/wekaexamples/gui/visualize/GenerateROC.java|stable-3.6), [developer](https://git.cms.waikato.ac.nz/weka/weka/-/tree/main/trunk/wekaexamples/src/main/java/wekaexamples/gui/visualize/GenerateROC.java))

