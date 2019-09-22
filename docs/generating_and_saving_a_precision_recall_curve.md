The following Java class evaluates a NaiveBayes classifier using cross-validation with a dataset provided by the user and saves a precision-recall curve for the first class label as a JPEG file, based on a user-specified file name.

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
 * Generates and saves a precision-recall curve. Uses a cross-validation
 * with NaiveBayes to make the curve.
 *
 * @author FracPete
 * @author Eibe Frank
 */
public class SavePrecisionRecallCurve {

  /**
   * takes two arguments: dataset in ARFF format (expects class to
   * be last attribute) and name of file with output
   */
  public static void main(String[] args) throws Exception {

    // load data
    Instances data = new Instances(new BufferedReader(new FileReader(args[0])));
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
    PlotData2D tempd = new PlotData2D(result);

    // specify which points are connected
    boolean[] cp = new boolean[result.numInstances()];
    for (int n = 1; n < cp.length; n++)
      cp[n] = true;
    tempd.setConnectPoints(cp);
    // add plot
    vmc.addPlot(tempd);

    // We want a precision-recall curve
    vmc.setXIndex(result.attribute("Recall").index());
    vmc.setYIndex(result.attribute("Precision").index());

    // Make window with plot but don't show it
    JFrame jf =  new JFrame();
    jf.setSize(500,400);
    jf.getContentPane().add(vmc);
    jf.pack();

    // Save to file specified as second argument (can use any of
    // BMPWriter, JPEGWriter, PNGWriter, PostscriptWriter for different formats)
    JComponentWriter jcw = new JPEGWriter(vmc.getPlotPanel(), new File(args[1]));
    jcw.toOutput();
    System.exit(1);
  }
}
```

# See also
* [ROC curves](roc_curves.md)
* [Visualizing ROC curve](visualization/visualizing_roc_curve.md)
* [Plotting multiple ROC curves](plotting_multiple_roc_curves.md)
> 
## Version
Needs the developer version >=3.5.1 or 3.6.x