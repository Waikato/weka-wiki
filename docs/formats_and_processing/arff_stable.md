An ARFF (Attribute-Relation File Format) file is an ASCII text file that describes a list of instances sharing a set of attributes. 

# Overview
ARFF files have two distinct sections. The first section is the **Header** information, which is followed the **Data** information.

The **Header** of the ARFF file contains the name of the relation, a list of the attributes (the columns in the data), and their types. An example header on the standard IRIS dataset looks like this:

```
   % 1. Title: Iris Plants Database
   % 
   % 2. Sources:
   %      (a) Creator: R.A. Fisher
   %      (b) Donor: Michael Marshall (MARSHALL%PLU@io.arc.nasa.gov)
   %      (c) Date: July, 1988
   % 
   @RELATION iris
   
   @ATTRIBUTE sepallength  NUMERIC
   @ATTRIBUTE sepalwidth   NUMERIC
   @ATTRIBUTE petallength  NUMERIC
   @ATTRIBUTE petalwidth   NUMERIC
   @ATTRIBUTE class        {Iris-setosa,Iris-versicolor,Iris-virginica}
```

The **Data** of the ARFF file looks like the following:

```
   @DATA
   5.1,3.5,1.4,0.2,Iris-setosa
   4.9,3.0,1.4,0.2,Iris-setosa
   4.7,3.2,1.3,0.2,Iris-setosa
   4.6,3.1,1.5,0.2,Iris-setosa
   5.0,3.6,1.4,0.2,Iris-setosa
   5.4,3.9,1.7,0.4,Iris-setosa
   4.6,3.4,1.4,0.3,Iris-setosa
   5.0,3.4,1.5,0.2,Iris-setosa
   4.4,2.9,1.4,0.2,Iris-setosa
   4.9,3.1,1.5,0.1,Iris-setosa
```

Lines that begin with a `%` are comments. The **@RELATION**, **@ATTRIBUTE** and **@DATA** declarations are case insensitive.

# Examples
Several well-known machine learning datasets are distributed with Weka in the `$WEKAHOME/data` directory as ARFF files.

## The ARFF Header Section 
The ARFF Header section of the file contains the relation declaration and attribute declarations.

## The @relation Declaration
The relation name is defined as the first line in the ARFF file. The format is:

```
    @relation [relation-name]
```

where *[relation-name]* is a string. The string must be quoted if the name includes spaces. Furthermore, relation names or attribute names (see below) cannot begin with

* a character below \u0021
* '{', '}', ',', or '%' 

Moreover, it can only begin with a single or double quote if there is a corresponding quote at the end of the name.

## The @attribute Declarations
Attribute declarations take the form of an ordered sequence of **@attribute** statements. Each attribute in the data set has its own **@attribute** statement which uniquely defines the name of that attribute and its data type. The order the attributes are declared indicates the column position in the data section of the file. For example, if an attribute is the third one declared then Weka expects that all that attributes values will be found in the third comma delimited column.

The format for the **@attribute** statement is:

```
    @attribute [attribute-name] [datatype]
```

where the *[attribute-name]* must adhere to the constraints specified in the above section on the @relation declaration.

The [datatype] can be any of the four types supported by Weka:

* *numeric*
* *integer* is treated as *numeric*
* *real* is treated as *numeric*
* [nominal-specification]
* *string*
* *date* [date-format]
* *relational* for multi-instance data (for future use)

  where *[nominal-specification]* and *[date-format]* are defined below. The keywords **numeric**, **real**, **integer**, **string** and **date** are case insensitive.

### Numeric attributes 
Numeric attributes can be real or integer numbers.

### Nominal attributes
Nominal values are defined by providing an [nominal-specification] listing the possible values: {[nominal-name1], [nominal-name2], [nominal-name3], ...}

For example, the class value of the Iris dataset can be defined as follows:

```
    @ATTRIBUTE class        {Iris-setosa,Iris-versicolor,Iris-virginica}
```

Values that contain spaces must be quoted.

### String attributes
String attributes allow us to create attributes containing arbitrary textual values. This is very useful in text-mining applications, as we can create datasets with string attributes, then write Weka Filters to manipulate strings (like `StringToWordVectorFilter`). String attributes are declared as follows:

```
    @ATTRIBUTE LCC    string
```

### Date attributes
Date attribute declarations take the form:

```
    @attribute [name] date [[date-format]]
```

where [name] is the name for the attribute and *[date-format]* is an optional string specifying how date values should be parsed and printed (this is the same format used by `SimpleDateFormat`). The default format string accepts the ISO-8601 combined date and time format: `yyyy-MM-dd'T'HH:mm:ss`. Check out the Javadoc of the [java.text.SimpleDateFormat](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html) class for supported character patterns.

Dates must be specified in the data section as the corresponding string representations of the date/time (see example below).

### Relational attributes 
Relational attribute declarations take the form:

```
    @attribute [name] relational
      [further attribute definitions]
    @end [name]
```

For the multi-instance dataset *MUSK1* the definition would look like this (**"..."** denotes an omission):

```
 @attribute molecule_name {MUSK-jf78,...,NON-MUSK-199}
 @attribute bag relational
   @attribute f1 numeric
   ...
   @attribute f166 numeric
 @end bag
 @attribute class {0,1}
 ...
```

## The ARFF Data Section
The ARFF Data section of the file contains the data declaration line and the actual instance lines.

## The @data Declaration
The **@data** declaration is a single line denoting the start of the data segment in the file. The format is:

```
    @data
```

## The instance data
Each instance is represented on a single line, with carriage returns denoting the end of the instance.  A percent sign (%) introduces a comment, which continues to the end of the line.

Attribute values for each instance can be delimited by commas or tabs. A comma/tab may be followed by zero or more spaces. Attribute values must appear in the order in which they were declared in the header section (i.e., the data corresponding to the nth **@attribute** declaration is always the nth field of the attribute).

A missing value is represented by a single question mark, as in:

```
    @data
    4.4,?,1.5,?,Iris-setosa
```

Values of string and nominal attributes are case sensitive, and any that contain space or the comment-delimiter character % must be quoted.  (The code suggests that double-quotes are acceptable and that a backslash will escape individual characters.)  An example follows:

```
    @relation LCCvsLCSH
    
    @attribute LCC string
    @attribute LCSH string
    
    @data
    AG5,   'Encyclopedias and dictionaries.;Twentieth century.'
    AS262, 'Science -- Soviet Union -- History.'
    AE5,   'Encyclopedias and dictionaries.'
    AS281, 'Astronomy, Assyro-Babylonian.;Moon -- Phases.'
    AS281, 'Astronomy, Assyro-Babylonian.;Moon -- Tables.'
```

Dates must be specified in the data section using the string representation specified in the attribute declaration. For example:

```
    @RELATION Timestamps
    
    @ATTRIBUTE timestamp DATE "yyyy-MM-dd HH:mm:ss"
    
    @DATA 
    "2001-04-03 12:12:12"
    "2001-05-03 12:59:55"
```

Relational data must be enclosed within double quotes *"*. For example an instance of the *MUSK1* dataset (*"..."* denotes an omission):

```
 MUSK-188,"42,...,30",1
```

# Sparse ARFF files
Sparse ARFF files are very similar to ARFF files, but data with value 0 are not be explicitly represented.

Sparse ARFF files have the same header (i.e **@relation** and **@attribute** tags) but the data section is different. Instead of representing each value in order, like this:

```
    @data
    0, X, 0, Y, 'class A'
    0, 0, W, 0, 'class B'
```

the non-zero attributes are explicitly identified by attribute number and their value stated, like this:

```
    @data
    {1 X, 3 Y, 4 'class A'}
    {2 W, 4 'class B'}
```

Each instance is surrounded by curly braces, and the format for each entry is: [index] [space] [value] where index is the attribute index (starting from 0).

Note that the omitted values in a sparse instance are **0**, they are not "missing" values! If a value is unknown, you must explicitly represent it with a question mark (?).

**Warning:** There is a known problem saving SparseInstance objects from datasets that have string attributes. In Weka, string and nominal data values are stored as numbers; these numbers act as indexes into an array of possible attribute values (this is very efficient). However, the first string value is assigned index 0: this means that, internally, this value is stored as a 0. When a SparseInstance is written, string instances with internal value 0 are not output, so their string value is lost (and when the arff file is read again, the default value 0 is the index of a different string value, so the attribute value appears to change). To get around this problem, add a dummy string value at index 0 that is never used whenever you declare string attributes that are likely to be used in SparseInstance objects and saved as Sparse ARFF files.

# Instance weights in ARFF files
A weight can be associated with an instance in a standard ARFF file by appending it to the end of the line for that instance and enclosing the value in curly braces. E.g:

```
    @data
    0, X, 0, Y, 'class A', {5}
```

For a sparse instance, this example would look like:

```
    @data
    {1 X, 3 Y, 4 'class A'}, {5}
```

Note that any instance without a weight value specified is assumed to have a weight of 1 for backwards compatibility.

# See also
* [Add weights to dataset](../add_weights_to_dataset.md)
* [ARFF Syntax Highlighting](arff_syntax.md) for various editors

# Links
* [ISO-8601](http://www.iso.org/iso/en/prods-services/popstds/datesandtime.html)
* Javadoc of [java.text.SimpleDateFormat](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html) (lists the supported character patterns)
* ANTLR syntax by Staal A. Vinterbo [arff.g](../files/arff.g)

