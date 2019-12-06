
In the following one can find some information of how to use Weka for [text categorization](http://en.wikipedia.org/wiki/document_classification).

# Import

Weka needs the data to be present in [ARFF](formats_and_processing/arff.md) or [XRFF](formats_and_processing/xrff.md) format in order to perform any classification tasks.


## Directories

One can transform the text files with the following tools into ARFF format (depending on the version of Weka you are using):

* **[TextDirectoryToArff](formats_and_processing/arff_from_text_collections.md)** tool (3.4.x and >= 3.5.3)
> this Java class transforms a directory of files into an ARFF file
* **[TextDirectoryLoader](https://weka.sourceforge.io/doc.stable-3-8/weka/core/converters/TextDirectoryLoader.html)** converter (> 3.5.3)
> this converter is based on the *[TextDirectoryToArff](formats_and_processing/arff_from_text_collections.md)* tool and located in the `weka.core.converters` package

Example directory layout for **TextDirectoryLoader**:

```
 ...
 |
 +- text_example
    |
    +- class1
    |  |
    |  + file1.txt
    |  |
    |  + file2.txt
    |  |
    |  ...
    |
    +- class2
    |  |
    |  + another_file1.txt
    |  |
    |  + another_file2.txt
    |  |
    |  ...
```
The above directory structure can be turned into an ARFF file like this:

```bash
java weka.core.converters.TextDirectoryLoader -dir text_example > text_example.arff
```

## CSV files

CSV files can be imported in Weka easily via the Weka Explorer or via commandline via the `CSVLoader` class:

```bash
 java weka.core.converters.CSVLoader file.csv > file.arff
```

By default, non-numerical attributes get imported as *NOMINAL* attributes, which is not necessarily desired for textual data, especially if one wants to use the [StringToWordVector](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/unsupervised/attribute/StringToWordVector.html) filter. In order to change the attribute to *STRING*, one can run the `NominalToString` filter (package `weka.filters.unsupervised.attribute`) on the data, specifying the attribute index or range of indices that should be converted (NB: 
this filter does **not** exclude the class attribute from conversion!). In order to retain the attribute types, one needs to save the file in [ARFF](formats_and_processing/arff.md) or [XRFF](formats_and_processing/xrff.md) format (or in the compressed version of these formats).

## Third-party tools

* [TagHelper Tools](http://www.cs.cmu.edu/~cprose/TagHelper.html), which allows one to transform texts into vectors of stemmed or unstemmed unigrams, bigrams, part-of-speech bigrams, and some user defined features, and then saves this representation to [ARFF](formats_and_processing/arff.md). Currently processes English, German, and Chinese. Spanish and Portugese are in progress.

# Working with textual data

## Conversion

Most classifiers in Weka cannot handle *String* attributes. For these learning schemes one has to process the data with appropriate filters, e.g., the [StringToWordVector](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/unsupervised/attribute/StringToWordVector.html) filter which can perform [TF/IDF transformation](http://en.wikipedia.org/wiki/tf%e2%80%93idf).

The `StringToWordVector` filter places the class attribute of the generated output data at the beginning. In case you'd to like to have it as last attribute again, you can use the [Reorder](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/unsupervised/attribute/Reorder.html) filter with the following setup:

```bash
weka.filters.unsupervised.attribute.Reorder -R 2-last,first
```

And with the [MultiFilter](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/MultiFilter.html) you can also apply both filters in one go, instead of subsequently. Makes it easier in the Explorer for instance.

## Stopwords

The [StringToWordVector](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/unsupervised/attribute/StringToWordVector.html) filter can also work with a different stopword list than the built-in one (based on the Rainbow system). One can use the `-stopwords` option to load the external stopwords file. The format for such a stopword file is one stopword per line, lines starting with '#' are interpreted as comments and ignored.

**Note:** There was a bug in Weka 3.5.6 (which introduced the support of external stopwords lists), which ignored the external stopwords list. Later versions or [snapshot](snapshots.md)s from 21/07/2007 on will work correctly.

# UTF-8

In case you are working with text files containing non-ASCII characters, e.g., Arabic, you might encounter some display problems under Windows. Java was designed to display [UTF-8](http://en.wikipedia.org/wiki/utf-8), which should include arabic characters. By default, Java uses [code page 1252](http://en.wikipedia.org/wiki/cp1252) under Windows, which garbles the display of other characters. In order to fix this, you will have to modify the java command-line with which you start up Weka:

```bash
  java -Dfile.encoding=utf-8 -classpath ...
```
The `-Dfile.encoding=utf-8` tells Java to explicitly use [UTF-8](http://en.wikipedia.org/wiki/utf-8) encoding instead of the default [CP1252](http://en.wikipedia.org/wiki/cp1252).
If you are starting Weka via start menu and you use a recent version (at least 3.5.8 or 3.4.13), then you will just have to modify the `fileEncoding` placeholder in the `RunWeka.ini` accordingly.

# Examples

* [text_example.zip](files/text_example.zip) - contains a directory structure and example files that can be imported with the `TextDirectoryLoader` converter.
* [TextCategorizationTest.java](files/TextCategorizationTest.java) - uses the `TextDirectoryLoader` converter to turn a directory structure into a dataset, applies the `StringToWordVector` and builds a classifier with the filtered data.

# See also

* [Batch filtering](batch_filtering.md) - for generating a test set with the same dictionary as the training set
* [All text categorization articles](https://waikato.github.io/weka-wiki/search.html?q=text+categorization)

# Links

* Javadoc
	* [StringToWordVector](https://weka.sourceforge.io/doc.stable-3-8/weka/filters/unsupervised/attribute/StringToWordVector.html)
	* [TextDirectoryLoader](https://weka.sourceforge.io/doc.stable-3-8/weka/core/converters/TextDirectoryLoader.html)