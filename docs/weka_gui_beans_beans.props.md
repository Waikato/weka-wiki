

# File
`weka/gui/beans/Beans.props`

# Description
Properties file that customizes the KnowledgeFlow.

# Version
* \>= 3.3.2

# Fields
* `weka.gui.beans.KnowledgeFlow.standardToolBars`
> list of standard toolbars (containing bean tools that do not wrap weka base class types)
* `*.order`
> toolbar ordering information for wrapper types
* `*.alias`
> toolbar naming aliases for weka algorithm classes
* GUI behavior (>= 3.5.1)
	* `ScrollBarIncrementLayout`
>> the increment for scrollbars using the mouse's scroll-wheel in the layout area
	* `ScrollBarIncrementComponents`
>> the increment for scrollbars using the mouse's scroll-wheel in the component area (i.e., the toolbars)
	* `FlowWidth`
>> the size of the layout area
	* `FlowHeight`
>> the size of the layout area
	* `PreferredExtension`
>> the preferred file extension (and therefore format) for saving a flow layout
	* `UserComponentsInXML`
>> whether the user components ("meta"-beans) are saved in XML (i.e., `.kfml`) or not
* Colours (> 3.5.7)
	* `weka.gui.beans.StripChart.backgroundColour`
>> the background color of the StripChart, default is *black* (can use R,G,B format)
	* `weka.gui.beans.StripChart$LegendPanel.borderColour`
>> the color of the text on the legend's border, default is *blue* (can use R,G,B format)

# See also
* [Properties File](properties_file.md)
