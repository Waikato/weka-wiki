

# File
`weka/gui/GenericPropertiesCreator.props`

# Description
Lists all the packages to look in for subclasses of a certain superclass to be displayed in the [GenericObjectEditor](generic_object_editor.md).

**Note:** Weka 3.5.8 turned the automatic discovery off by default. In this case, the [weka/gui/GenericObjectEditor.props](weka_gui_generic_object_editor.props.md) is used again.

# Version
* \>= 3.4.4

# Fields
* enable/disable dynamic discovery (> 3.5.5)
> `UseDynamic=true|false`
* Format (a *backslash* at the end continues the package list on the next line)
> `<superclass>=<package>[,package[,...]]`
* Filter example
```ini
 weka.filters.Filter= \
  weka.filters, \
  weka.filters.supervised.attribute, \
  weka.filters.supervised.instance, \
  weka.filters.unsupervised.attribute, \
  weka.filters.unsupervised.instance
```
* Classifier example
```ini
 weka.classifiers.Classifier=\
  weka.classifiers.bayes,\
  weka.classifiers.functions,\
  weka.classifiers.lazy,\
  weka.classifiers.meta,\
  weka.classifiers.meta.nestedDichotomies,\
  weka.classifiers.mi,\
  weka.classifiers.misc,\
  weka.classifiers.trees,\
  weka.classifiers.rules
```

# See also
* [GenericObjectEditor](generic_object_editor.md) (explains how to add new schemes)
* [Properties file](properties_file.md)
