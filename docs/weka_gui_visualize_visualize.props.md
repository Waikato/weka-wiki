

# File
`weka/gui/visualize/Visualize.props`

# Description
Customizes display of plots and certain curves in the GUI.

# Version
* \>= 3.1.9

# Fields
* `weka.gui.visualize.precision`
> Maximum precision for numeric values
* `weka.gui.visualize.Plot2D.axisColour`
> Colour for the axis in the 2D plot (can use R,G,B format)
* `weka.gui.visualize.Plot2D.backgroundColour`
> Colour for the background of the 2D plot (can use R,G,B format)
* `weka.gui.visualize.VisualizePanel.displayAttributeBars`
> Display the list of one dimensional attribute visualizations
* `weka.gui.visualize.AttributePanel.barColour`
> Colour for the background of the attribute bars in the AttributePanel (can use R,G,B format)
* `weka.gui.visualize.Plot2D.instanceInfoFrame` (developer version later than 3.5.8 or [snapshot](snapshots.md), not in stable-3.6)
> Lists the classname for displaying the instance info, e.g., when visualizing the classifier errors in the Explorer. Custom classes only need to be derived from `javax.swing.JFrame` and implement the `weka.gui.visualize.InstanceInfo` interface.
* Threshold curve plots
	* `weka.gui.visualize.ThresholdVisualizePanel.ThresholdCurve.XDimension`
	* `weka.gui.visualize.ThresholdVisualizePanel.ThresholdCurve.YDimension`
	* `weka.gui.visualize.ThresholdVisualizePanel.ThresholdCurve.ColourDimension`
* Cost curve plots
	* `weka.gui.visualize.VisualizePanel.CostCurve.XDimension`
	* `weka.gui.visualize.VisualizePanel.CostCurve.YDimension`
	* `weka.gui.visualize.VisualizePanel.CostCurve.ColourDimension`
* Margin curve plots
	* `weka.gui.visualize.VisualizePanel.MarginCurve.XDimension`
	* `weka.gui.visualize.VisualizePanel.MarginCurve.YDimension`
	* `weka.gui.visualize.VisualizePanel.MarginCurve.ColourDimension`

# Notes
* Recognized **color names**
	* black
	* blue
	* cyan
	* darkGray
	* gray
	* green
	* lightGray
	* magenta
	* orange
	* pink
	* red
	* white
	* yellow
* **R,G,B format**
> The RGB format is a comma-separated list three integer values (values ranging from 0-255) for RED, GREEN and BLUE.

# See also
* [Properties File](properties_file.md)
