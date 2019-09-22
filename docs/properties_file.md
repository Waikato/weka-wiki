

# General
A properties file is a simple text file with this structure:

> `<key>=<value>`

Notes:

* Comments start with the hash sign `#`.
* Backslashes within values need to be doubled (the backslashes get interpreted already when a property is read).

To make a rather long property line more readable, one can use a backslash to continue on the next line. The Filter property, e.g., looks like this:

```bash
weka.filters.Filter= \
> weka.filters.supervised.attribute, \
> weka.filters.supervised.instance, \
> weka.filters.unsupervised.attribute, \
> weka.filters.unsupervised.instance
```

# Precedence
The Weka property files (extension *.props*) are searched for in the following order:

* current directory
* (< Weka 3.7.2) the user's home directory (see FAQ [Where is my home directory located?](faqs/home_directory_location.md) for more information)
* (>= Weka 3.7.2) $WEKA_HOME/props (the default value for WEKA_HOME is user's home directory/wekafiles).
* the class path (normally the `weka.jar` file) 

If WEKA encounters those files it only supplements the properties, never overrides them. In other words, a property in the property file of the current directory has a higher precedence than the one in the user's home directory.

**Note:** Under [Cywgin](http://cygwin.com/), the home directory is still the Windows one, since the java installation will be still one for Windows.

# How to modify a .props file?
It is quite possible, that the default setup of WEKA is not to your liking and that you want to tweak it a little bit. The use of `.props` files instead of hard-coding makes it quite easy to modify WEKA's behavior. As example, we are modifying the background color of the 2D plots in the Explorer, changing it to *dark gray*. The responsible `.props` file is [weka/gui/visualize/Visualize.props](weka_gui_visualize_visualize.props.md).
These are the necessary steps:

* close WEKA
* extract the `.props` file from the `weka.jar`, using an archive manager that can handle ZIP files (e.g., [7-Zip](http://7-zip.org/) under Windows)
* place this `.props` file in your home directory (see FAQ [Where is my home directory located?](where is my home directory located?.md) on how to determine your home directory), or for Weka 3.7.2 or higher place this `.props` file in $WEKA_HOME/props (the default value of WEKA_HOME is user's home directory/wekafiles)
* open this `.props` with a text editor (**NB:** Notepad under Windows might not handle the Unix line-endings correctly!)
* navigate to the property `weka.gui.visualize.Plot2D.backgroundColour` and change the color after the equal sign ("=") to `darkGray` (the article about [weka/gui/visualize/Visualize.props](weka_gui_visualize_visualize.props.md) lists all possible colors)
* save the file and restart WEKA

# Notes
* **Escaping**
> Backslashes in values need to be escaped (i.e., doubled), otherwise they get interpreted as character sequence.
> E.g., "is\this" will be interpreted as "is<TAB>his". Correctly escaped, this would read as "is\\this".

# See also
Further information about specific props files:

* [weka/core/Capabilities.props](weka_core_capabilities.props.md)
* [weka/core/logging/Logging.props](weka_core_logging_logging.props.md)
* [weka/experiment/DatabaseUtils.props](weka_experiment_database_utils.props.md)
* [weka/gui/GenericObjectEditor.props](weka_gui_generic_object_editor.props.md)
* [weka/gui/GUIEditors.props](weka_gui_gui_editors.props.md)
* [weka/gui/GenericPropertiesCreator.props](weka_gui_generic_properties_creator.props.md)
* [weka/gui/GenericPropertiesCreator.excludes](weka_gui_generic_properties_creator.excludes.md)
* [weka/gui/LookAndFeel.props](weka_gui_look_and_feel.props.md)
* [weka/gui/MemoryUsage.props](weka_gui_memory_usage.props.md)
* [weka/gui/SimpleCLI.props](weka_gui_simple_cli.props.md)
* [weka/gui/beans/Beans.props](weka_gui_beans_beans.props.md)
* [weka/gui/experiment/Experimenter.props](weka_gui_experiment_experimenter.props.md)
* [weka/gui/explorer/Explorer.props](weka_gui_explorer_explorer.props.md)
* [weka/gui/scripting/Groovy.props](weka_gui_scripting_groovy.props.md)
* [weka/gui/scripting/Jython.props](weka_gui_scripting_jython.props.md)
* [weka/gui/treevisualizer/TreeVisualizer.props](weka_gui_tree_visualizer_tree_visualizes.props.md)
* [weka/gui/visualize/Visualize.props](weka_gui_visualize_visualize.props.md)
