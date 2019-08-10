

# Introduction
Weka supports stemming algorithms in the developer version. The stemming algorithms are located in the following package:

```
 weka.core.stemmers
```
Currently, the Lovins Stemmer (+ iterated version) and support for the Snowball stemmers are included.

# Snowball stemmers

Weka contains a wrapper class for the [Snowball](http://snowball.tartarus.org/) stemmers (containing the Porter stemmer and several other stemmers for different languages). The relevant class is weka.core.stemmers.SnowballStemmer.

The Snowball classes are not included, they only have to be present in the classpath. The reason for this is, that the Weka team doesn't have to watch out for new versions of the stemmers and update them.

There are **three** ways of getting hold of the Snowball stemmers:

* For Weka 3.7.x you can install an [unofficial package](packages/unofficial.md)
* You can add the **pre-compiled [snowball-20051019.jar](files/snowball-20051019.jar) archive** to your classpath and you're set.
> (based on source code from 2005-10-19, compiled 2005-10-22)
* You can **compile the stemmers yourself** with the newest sources.
> Just download the [snowball-20051019.zip](files/snowball-20051019.zip).
> **Note:** the *patch* target is specific to the source code from 2005-10-19.

# PTStemmer
[PTStemmer](http://code.google.com/p/ptstemmer/) is a stemmer library for Portuguese developed by Pedro Oliveira.

In order to use this library:

* you can install the [unofficial package](packages/unofficial.md) when using Weka 3.7.x
* you just need to download the [ptstemmer.jar](files/ptstemmer.jar) and add them to your classpath.

The source code of the wrapper project is also available: 
[ptstemmer-weka-src-20091105.tar.gz](files/ptstemmer-weka-src-20091105.tar.gz).
**NB:** the source code and the resulting jars are based on version 1.0 of the PTStemmer library.

# Using stemmers

The stemmers can either be used:

* from commandline
* within the `StringToWordVector` (package `weka.filters.unsupervised.attribute`)

## Commandline

All stemmers support the following options:

* `-h`
> for displaying a brief help
* `-i <input-file>`
> The file to process
* `-o <output-file>`
> The file to output the processed data to (default *stdout*)
* `-l`
> Uses lowercase strings, i.e. the input is automatically converted to lower case

## StringToWordVector

Just use the [GenericObjectEditor](generic_object_editor.md) to choose the right stemmer and the desired options (if the stemmer offers these).

# Adding new stemmers

You can easily add new stemmers, if you follow these guidelines (for use in the [GenericObjectEditor](generic_object_editor.md)):

* they should be located in the `weka.core.stemmers` package and
* they must implement the interface `weka.core.stemmers.Stemmer`.

# Links

* [Snowball homepage](http://snowball.tartarus.org/)
* [ANT homepage](http://ant.apache.org/)
