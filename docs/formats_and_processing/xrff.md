
The **XRFF** (e**X**tensible attribute-**R**elation **F**ile **F**ormat) is an XML-based extension of the [ARFF](arff.md) format.

# File extensions
* **.xrff**
> the default extension of *XRFF* files
* **.xrff.gz**
> the extension for **gzip** compressed *XRFF* files (see [Compression](xrff.md#compression) for more details)

# Comparison
# ARFF
In the following a snippet of the UCI dataset *iris* in **ARFF** format:

```text
 @relation iris
 
 @attribute sepallength numeric
 @attribute sepalwidth numeric
 @attribute petallength numeric
 @attribute petalwidth numeric
 @attribute class {Iris-setosa,Iris-versicolor,Iris-virginica}
 
 @data
 5.1,3.5,1.4,0.2,Iris-setosa
 4.9,3,1.4,0.2,Iris-setosa
 ...
```

## XRFF
And the same dataset represented as **XRFF** file:

```xml
 <?xml version="1.0" encoding="utf-8"?>
 
 <!DOCTYPE dataset
 [
    <!ELEMENT dataset (header,body)>
    <!ATTLIST dataset name CDATA #REQUIRED>
    <!ATTLIST dataset version CDATA "3.5.4">
 
    <!ELEMENT header (notes?,attributes)>
    <!ELEMENT body (instances)>
    <!ELEMENT notes ANY>   <!--  comments, information, copyright, etc. -->
 
    <!ELEMENT attributes (attribute+)>
    <!ELEMENT attribute (labels?,metadata?,attributes?)>
    <!ATTLIST attribute name CDATA #REQUIRED>
    <!ATTLIST attribute type (numeric|date|nominal|string|relational) #REQUIRED>
    <!ATTLIST attribute format CDATA #IMPLIED>
    <!ATTLIST attribute class (yes|no) "no">
    <!ELEMENT labels (label*)>   <!-- only for type "nominal" -->
    <!ELEMENT label ANY>
    <!ELEMENT metadata (property*)>
    <!ELEMENT property ANY>
    <!ATTLIST property name CDATA #REQUIRED>
 
    <!ELEMENT instances (instance*)>
    <!ELEMENT instance (value*)>
    <!ATTLIST instance type (normal|sparse) "normal">
    <!ATTLIST instance weight CDATA #IMPLIED>
    <!ELEMENT value (#PCDATA|instances)*>
    <!ATTLIST value index CDATA #IMPLIED>   <!-- 1-based index (only used for instance format "sparse") -->
    <!ATTLIST value missing (yes|no) "no">
 ]
 >
 
 <dataset name="iris" version="3.5.3">
    <header>
       <attributes>
          <attribute name="sepallength" type="numeric"/>
          <attribute name="sepalwidth" type="numeric"/>
          <attribute name="petallength" type="numeric"/>
          <attribute name="petalwidth" type="numeric"/>
          <attribute class="yes" name="class" type="nominal">
             <labels>
                <label>Iris-setosa</label>
                <label>Iris-versicolor</label>
                <label>Iris-virginica</label>
             </labels>
          </attribute>
       </attributes>
    </header>
 
    <body>
       <instances>
          <instance>
             <value>5.1</value>
             <value>3.5</value>
             <value>1.4</value>
             <value>0.2</value>
             <value>Iris-setosa</value>
          </instance>
          <instance>
             <value>4.9</value>
             <value>3</value>
             <value>1.4</value>
             <value>0.2</value>
             <value>Iris-setosa</value>
          </instance>
          ...
       </instances>
    </body>
 </dataset>
```

# Sparse format
The *XRFF* format also supports a sparse data representation. Even though the iris dataset does not contain sparse data, the above example will be used here to illustrate the sparse format:

```xml
 ...
 <instances>
    <instance type="sparse">
       <value index="1">5.1</value>
       <value index="2">3.5</value>
       <value index="3">1.4</value>
       <value index="4">0.2</value>
       <value index="5">Iris-setosa</value>
    </instance>
    <instance type="sparse">
       <value index="1">4.9</value>
       <value index="2">3</value>
       <value index="3">1.4</value>
       <value index="4">0.2</value>
       <value index="5">Iris-setosa</value>
    </instance>
    ...
 </instances>
 ...
```
In contrast to the *normal* data format, each sparse *instance* tag contains a *type* attribute with the value *sparse*:

```xml
 <instance type="sparse">
```
And each *value* tag needs to specify the *index* attribute, which contains the 1-based index of this value. 
```xml
 <value index="1">5.1</value>
```

# Compression
Since the XML representation takes up considerably more space than the rather compact *[ARFF](arff.md)* format, one can also compress the data via **gzip**. Weka automatically recognizes a file being gzip compressed, if the file's extension is *.xrff.gz* instead of *.xrff*.

The Weka Explorer now allows to load/save compressed and uncompressed XRFF files (this applies also to [ARFF](arff.md) files).

# Additional features
In addition to all the features of the [ARFF](arff.md) format, the *XRFF* format contains the following additional features:

* class attribute specification
* attribute weights
* instance weights

## Class attribute specification 
Via the `class="yes"` attribute in the attribute specification in the header, one can define which attribute should act as class attribute. A feature that can be used on the command line as well as in the Experimenter, which now can also load other data formats, and removing the limitation of the class attribute always having to be the last one.

Snippet from the iris dataset:

```xml
 <attribute **class="yes"** name="class" type="nominal">
```

## Attribute weights
Attribute weights are stored in an attributes meta-data tag (in the *header* section). Here's an example of the *petalwidth* attribute with a weight of 0.9:

```xml
 <attribute name="petalwidth" type="numeric">
     <metadata>
        <property name="weight">0.9</property>
     </metadata>
 </attribute>
```

## Instance weights 
Instance weights are defined via the *weight* attribute in each *instance* tag. By default, the weight is 1. Here's an example:

```xml
 <instance weight="0.75">
    <value>5.1</value>
    <value>3.5</value>
    <value>1.4</value>
    <value>0.2</value>
    <value>Iris-setosa</value>
 </instance>
```
