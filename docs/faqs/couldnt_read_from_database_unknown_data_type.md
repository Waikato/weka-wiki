Since there is a plethora of different databases out there, each with their own data types, it is impossible to define all of them beforehand. WEKA therefore comes with setups for different databases that allow you to run experiments without any additional tuning. But if you want to read database from a different data source, then it can happen that you have to tell WEKA how to import these data types.

Here is what to do:

* Extract the [weka/experiment/DatabaseUtils.props](../weka_experiment_database_utils.props.md) file from either the `weka.jar` or `weka-src.jar` and place it in your home directory.
* Finally, check out the section *Missing Datatypes* in the [Databases](../databases.md) article and add the missing data types accordingly.

**Notes:**

* `jar` files are just [ZIP files](http://en.wikipedia.org/wiki/zip_(file_format)). Just use an archive manager that can handle ZIP files to open them. Windows users can use [7-zip](http://7-zip.org/) for instance.
* More information on props files can be found in the [Properties file](../properties_file.md) article.
* If you don't know where to find your *home directory*, see FAQ [Where is my home directory located?](home_directory_location.md).