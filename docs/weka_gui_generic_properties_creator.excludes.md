

# File
`weka/gui/GenericPropertiesCreator.excludes`

# Description
List classes for corresponding superclasses in [GenericPropertiesCreator.props](weka_gui_generic_properties_creator.props.md) that shouldn't appear in the [GenericObjectEditor](generic_object_editor.md) popup tree.

# Version
* \>= 3.5.3

# Fields
* Format
> `<key>=<prefix>:<class>[,<prefix>:<class>]`
	* `<key>`
>> the key from [GenericPropertiesCreator.props](weka_gui_generic_properties_creator.props.md) (class or interface)
	* `<prefix>`
		* `S` ("Superclass"): 
any class derived from this one will be excluded
		* `I` ("Interface"): 
any class implementing this interface will be excluded
		* `C` ("Class"): 
exactly this class will be excluded
* Example
> `weka.experiment.ResultListener=I:weka.experiment.ResultProducer`

# See also
* [weka/gui/GenericPropertiesCreator.props](weka_gui_generic_properties_creator.props.md)
* [GenericObjectEditor](generic_object_editor.md)
* [Properties file](properties_file.md)
