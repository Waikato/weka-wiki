

# File
`weka/gui/LookAndFeel.props`

# Description
Defines with what theme Weka is displayed. By default Weka starts with the system default one but sometimes it can help to change this to get Weka going or present user interfaces more nicely.

# Version
* \>= 3.4.5
* \>= 3.5.0

# Fields
* `Theme`
> a few possible values
	* `javax.swing.plaf.metal.MetalLookAndFeel`
		* works on all platforms
		* fixes problem with Java 5/6 and Linux/Gnome
	* `com.sun.java.swing.plaf.windows.WindowsLookAndFeel`
		* theme for Win32
	* ...

# See also
* [Troubleshooting (GUIChooser starts but not Experimenter or Explorer)](gui_chooser_starts_but_not_experimenter_or_explorer.md)
* [Troubleshooting (KnowledgeFlow toolbars are empty)](knowledge_flow_toolbars_are_empty.md)
* [Properties file](properties_file.md)

