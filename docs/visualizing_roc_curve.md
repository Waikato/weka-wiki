The following class lets you display a previously saved [[ROC curves|ROC curve]], which also displays the [AUC](auc.md).

If you don't need the [AUC](auc.md), then you can also use this command to display the curve:

```bash
 java [CLASSPATH|-classpath <your-classpath>] weka.gui.visualize.VisualizePanel <file>
```

Source code:

```java
  import java.awt.*;
  import java.io.*;
  import javax.swing.*;
  import weka.core.*;
  import weka.classifiers.evaluation.*;
  import weka.gui.visualize.*;
  
  /**
   * Visualizes a previously saved ROC curve. Code taken from the 
   * <code>weka.gui.explorer.ClassifierPanel</code> - involved methods:
   * <ul>
   *    <li>visualize(String,int,int)</li>
   *    </li>visualizeClassifierErrors(VisualizePanel)</li>
   * </ul>
   *
   * @author FracPete
   */
  public class VisualizeROC {
    
    /**
     * takes one argument: previously saved ROC curve data (ARFF file)
     */
    public static void main(String[] args) throws Exception {
      Instances result = new Instances(
                            new BufferedReader(
                              new FileReader(args[0])));
      result.setClassIndex(result.numAttributes() - 1);
      ThresholdCurve tc = new ThresholdCurve();
      // method visualize
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
      // method visualizeClassifierErrors
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
* [Plotting multiple ROC curves](plotting_multiple_roc_curves.md) - also contains a Java example of plotting multiple ROC curves in a single plot

# Downloads
* [VisualizeROC.java](files/VisualizeROC.java) ([stable](https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-6/wekaexamples/src/main/java/wekaexamples/gui/visualize/VisualizeROC.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/gui/visualize/VisualizeROC.java))

