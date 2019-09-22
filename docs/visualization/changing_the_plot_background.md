The **default background** color for plots, e.g., for [ROC curves](../roc_curves.md), is **black**, which might not be convenient for screenshots.

Since the color is stored as a value in a [Properties file](../properties_file.md), you can easily change the color:

* extract the following file from the `weka.jar` or `weka-src.jar` file
```text
 weka/gui/visualize/Visualize.props
```
* place a copy of that file in your HOME directory (on *nix `$HOME`, on Windows `%USERPROFILE%`)
* edit the [Properties file](../properties_file.md) with any text editor
* replace the value of following key with a color of your liking, e.g., *white*
```text
 weka.gui.visualize.Plot2D.backgroundColour
```
* save the file and restart Weka

