# Data format
A description of the ARFF format can be found in the following articles:

* [ARFF (stable version)](arff_stable.md)
* [ARFF (developer version)](arff_developer.md)

Note how single quotes and spaces are handled:

* [Single Quotes in Labels of ARFF Files](single_quotes_in_labels_of_arff_files.md)
* [Spaces in Labels of ARFF Files](spaces_in_labels_of_arff_files.md)

# Creating an ARFF file
How to create an ARFF file on the fly, i.e., inside Java, you can find here:

* [Creating an ARFF file](creating_arff_file.md)

# CSV

CSV (comma separated value) files are able to be converted to ARFF format. See:

* [Converting CSV to ARFF](converting_csv_to_arff.md)

# XML and XRFF
There is an XML-based extension of the ARFF format. See:

* [XRFF](xrff.md)
* [XML](xml.md)
* [Load an XML BIF file](load_an_xml_bif_file.md)

# See also
* [ARFF Syntax Highlighting](arff_syntax.md) for various editors
* [ARFF From Text Collections](arff_from_text_collections.md)
* [Remove Attributes](remove_attributes.md)
* [Rename Attribute Values](rename_attribute_values.md)
* [Save Instances to ARFF](save_instances_to_arff.md)
* [Transferring and ARFF File into a Databse](transferring_an_arff_file_into_a_database.md)


# Links
* [ARFF2DB.py](http://code.activestate.com/recipes/440533/) - a Python script for importing an ARFF file into a database (similar functionality to the `weka.core.converters.DatabaseSaver` class)
