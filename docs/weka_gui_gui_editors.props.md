

# File
`weka/gui/GUIEditors.props`

# Description
Lists what classes are handled by which GUI editor. Formerly done statically (but centralized) by the class `weka.gui.GenericObjectEditor` or in versions older than 3.5.2 all over the place.

# Version
* > 3.5.3

# Fields
* Format
	* `<class>=<editor>`
>> e.g., `java.io.File=weka.gui.FileEditor`
	* `<class[]>=<editor>` (for arrays)
>> e.g., `java.lang.Object[]=weka.gui.GenericArrayEditor`
* Available editors
	* General editors
		* `weka.gui.GenericObjectEditor`
		* `weka.gui.GenericArrayEditor`
	* Specialized editors
		* `weka.gui.CostMatrixEditor`
		* `weka.gui.EnsembleSelectionLibraryEditor`
		* `weka.gui.FileEditor`
		* `weka.gui.SelectedTagEditor`
		* `weka.gui.SimpleDateFormatEditor`

# See also
* [GenericObjectEditor](generic_object_editor.md)
* [Properties File](properties_file.md)
