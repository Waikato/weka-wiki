

# File
`weka/gui/MemoryUsage.props`

# Description
Contains properties for the memory usage panel/frame, which can be launched from:

* `weka.gui.Main`
> *File -> Memory usage*
* `weka.gui.GUIChooser`
> click on the *Memory usage* button

# Version
* > 3.5.7

# Fields
* `Width`
> the width of the panel in pixel. The default is *400*.
* `Height`
> the height of the panel - normally not set, since the height of the garbage collector button is used
* `Left`, `Top`
> specify a fixed location on the screen in pixel. Both of these parameters have to be different from -1 to place the window. The default is *-1* for both, i.e., top-left corner of the screen.
* `BackgroundColor`
> defines the background color of the panel, can either be the name of a [standard Java color](https://docs.oracle.com/javase/7/docs/api/java/awt/Color.html) or an RGB triplet (= `R,G,B`). Default is *white*.
* `Interval`
> the refresh interval in milli-second. The default is *1000*.
* `Percentages`
> comma-separated list of percentage number that indicate when to switch colors. E.g., *70,80,90* (which is the default) indicates to change the color of the bar from the default one (see `DefaultColor`) as soon as the percentage reaches this threshold. For example, if the percentage reaches 70 percent, then the color specified with the key *70* is used (default is *yellow* for *70*). If it reaches 80 or more then *orange* is used (the default for *80* is *orange*) and everything above and including *90* will be displayed *red* (*red* is the default for *90*).
* `DefaultColor`
> the default color to display the percentage bar with, i.e., if the percentage is below the lowest percentage listed in `Percentages`.

# See also
* [Properties file](properties_file.md)
