Transactional data is often stored in databases by having a table with the transaction ID as the primary key. Individual items or elements of a given transaction may be split up over multiple rows in the table (each with the same ID). Data in this format needs to be converted to one row per transaction before it can be used to learn classifiers, association rules, clusterers etc. in WEKA. From WEKA 3.7.2 there is a package called [denormalize](http://weka.sourceforge.net/packageMetaData/denormalize/index.html) that contains a filter that can perform this kind of "flattening" process. The filter requires **1) that the data contains an ID field that uniquely identifies each separate transaction**, and **2) the data is already sorted in order of this ID field**. Here is an example scenario taken from the WEKA mailing list:

```text
Hi,
   I have data spanning multiple rows for an instance, such as below (User 1 span across multiple rows, User 2 as well).  Is it possible to use WEKA to cluster this dataset?  If not, any suggestion on how I should organize the data so that I can use WEKA to cluster this data? 
 User  ItemID  Sequence TimeSpent
 1     1       1        5
 1     2       2        1
 1     5       3        8
 1     6       4        12
 1     8       5        2

 2     1       1        7
 2     2       2        3
 2     3       3        3
 2     4       4        2
 2     5       5        7
```

In WEKA 3.7.2 there is a package called [denormalize](http://weka.sourceforge.net/packageMetaData/denormalize/index.html) that contains a filter for flattening transactional data. The first thing you'd have to do to your example above is to convert it into an ARFF file:

```text
@relation test
@attribute User numeric
@attribute ItemID numeric
@attribute Sequence numeric
@attribute TimeSpent numeric
@data
1, 1, 1, 5
1, 2, 2, 1
1, 5, 3, 8
1, 6, 4, 12
1, 8, 5, 2
2, 1, 1, 7
2, 2, 2, 3
2, 3, 3, 3
2, 4, 4, 2
2, 5, 5, 7
```

Next, you can run the `NumericToNominal` filter to convert the attributes that need to be coded as nominal (the User attribute is an ID and can stay either as numeric or nominal). Here I've converted all attributes except the ID to nominal:

`java weka.filters.unsupervised.attribute.NumericToNominal -R 2-last -i test.arff > test2.arff`

This results in:

```text
@relation test-weka.filters.unsupervised.attribute.NumericToNominal-R2-last

@attribute User numeric
@attribute ItemID {1,2,3,4,5,6,8}
@attribute Sequence {1,2,3,4,5}
@attribute TimeSpent {1,2,3,5,7,8,12}

@data

1,1,1,5
1,2,2,1
1,5,3,8
1,6,4,12
1,8,5,2
2,1,1,7
2,2,2,3
2,3,3,3
2,4,4,2
2,5,5,7
```

Now, assuming that the denormalize package is installed, and (**IMPORTANT**) that the data is already sorted in order of the ID attribute ("User" in this case):

`java weka.Run Denormalize -G first -i test2.arff > final.arff`

This results in:

```text
@attribute User numeric
@attribute ItemID_1 {f,t}
@attribute ItemID_2 {f,t}
@attribute ItemID_3 {f,t}
@attribute ItemID_4 {f,t}
@attribute ItemID_5 {f,t}
@attribute ItemID_6 {f,t}
@attribute ItemID_8 {f,t}
@attribute Sequence_1 {f,t}
@attribute Sequence_2 {f,t}
@attribute Sequence_3 {f,t}
@attribute Sequence_4 {f,t}
@attribute Sequence_5 {f,t}
@attribute TimeSpent_1 {f,t}
@attribute TimeSpent_2 {f,t}
@attribute TimeSpent_3 {f,t}
@attribute TimeSpent_5 {f,t}
@attribute TimeSpent_7 {f,t}
@attribute TimeSpent_8 {f,t}
@attribute TimeSpent_12 {f,t}

@data

1,t,t,f,f,t,t,t,t,t,t,t,t,t,t,f,t,f,t,t
2,t,t,t,t,t,f,f,t,t,t,t,t,f,t,t,f,t,f,f
```

Note, that for clustering/association rules you'd want to first remove the User ID attribute.

I've shown this as an example using the command-line interface. It can all be done from the Explorer as well of course. The Denormalize filter has options for aggregating any numeric attributes (not the ID) as well, so if you left (for example) the TimeSpent attribute as numeric, rather than converting it to nominal using `NumericToNominal`, then `Denormalize` can aggregate it (sum, average, max, min).
