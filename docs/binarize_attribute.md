Sometimes one wants to binarize a nominal attribute of a certain dataset by grouping all values except the one of interest together as a negation of this value. E.g., in the {{weather}} data the outlook attribute, where *sunny* is of interest and the other values, *rainy* and *overcast*, are grouped together as *not-sunny*.

Original dataset:

```
 @relation weather

 @attribute outlook {sunny, overcast, rainy}
 @attribute temperature real
 @attribute humidity real
 @attribute windy {TRUE, FALSE}
 @attribute play {yes, no}

 @data
 sunny,85,85,FALSE,no
 sunny,80,90,TRUE,no
 overcast,83,86,FALSE,yes
 rainy,70,96,FALSE,yes
 rainy,68,80,FALSE,yes
 rainy,65,70,TRUE,no
 overcast,64,65,TRUE,yes
 sunny,72,95,FALSE,no
 sunny,69,70,FALSE,yes
 rainy,75,80,FALSE,yes
 sunny,75,70,TRUE,yes
 overcast,72,90,TRUE,yes
 overcast,81,75,FALSE,yes
 rainy,71,91,TRUE,no
```

Desired output:

```
 @relation weather-sunny-and-not_sunny

 @attribute outlook {sunny,not_sunny}
 @attribute temperature numeric
 @attribute humidity numeric
 @attribute windy {TRUE,FALSE}
 @attribute play {yes,no}

 @data
 sunny,85,85,FALSE,no
 sunny,80,90,TRUE,no
 not_sunny,83,86,FALSE,yes
 not_sunny,70,96,FALSE,yes
 not_sunny,68,80,FALSE,yes
 not_sunny,65,70,TRUE,no
 not_sunny,64,65,TRUE,yes
 sunny,72,95,FALSE,no
 sunny,69,70,FALSE,yes
 not_sunny,75,80,FALSE,yes
 sunny,75,70,TRUE,yes
 not_sunny,72,90,TRUE,yes
 not_sunny,81,75,FALSE,yes
 not_sunny,71,91,TRUE,no
```

The Weka filter [NominalToBinary](http://weka.sourceforge.net/doc.dev/weka/filters/unsupervised/attribute/NominalToBinary.html) cannot be used directly, since it generates a new attribute for each value of the nominal attribute. As a postprocessing step one could delete all the attributes that are of no interest, but this is quite cumbersome.

The [Binarize.java](files/Binarize.java) class on the other hand generates directly several [ARFF](arff.md) out of a given one in the desired format.

# Download
* [Binarize.java](files/Binarize.java) ([stable](ttps://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/wekaexamples/src/main/java/wekaexamples/filters/Binarize.java), [developer](https://svn.cms.waikato.ac.nz/svn/weka/trunk/wekaexamples/src/main/java/wekaexamples/filters/Binarize.java)
