

Weka now supports [XML](http://www.w3c.org/xml/) (e**X**tensible **M**arkup **L**anguage) in several places:


# Command Line
WEKA now allows to start Classifiers and Experiments with the `-xml` option followed by a filename to retrieve the command line options from the XML file instead of the command line.

For such simple classifiers like e.g. J48 this looks like overkill, but as soon as one uses Meta-Classifiers or Meta-Meta-Classifiers the handling gets tricky and one spends a lot of time looking for missing quotes. With the hierarchical structure of XML files it is simple to plug in other classifiers by just exchanging tags.

The DTD for the XML options is quite simple:

```xml
 <!DOCTYPE options
 [
    <!ELEMENT options (option)*>
    <!ATTLIST options type CDATA "classifier">
    <!ATTLIST options value CDATA "">
    <!ELEMENT option (#PCDATA | options)*>
    <!ATTLIST option name CDATA #REQUIRED>
    <!ATTLIST option type (flag | single | hyphens | quotes) "single">
 ]
 >
```
The type attribute of the option tag needs some explanations. There are currently four different types of options in WEKA:

* **flag**
> The simplest option that takes no arguments, like e.g. the `-V` flag for inversing an selection.
```xml
 <option name="V" type="flag"/>
```
* **single**
> The option takes exactly one parameter, directly following after the option, e.g., for specifying the trainings file with `-t somefile.arff`. Here the parameter value is just put between the opening and closing tag. Since single is the default value for the type  tag we don't need to specify it explicitly.
```xml
 <option name="t">somefile.arff</option>
```
* **hyphens**
> Meta-Classifiers like `AdaBoostM1` take another classifier as option with the `-W` option, where the options for the base classifier follow after the `--`. And here it is where the fun starts;  
where to put parameters for the base classifier if the Meta-Classifier itself is a base classifier for another Meta-Classifier?
> E.g., does `-W weka.classifiers.trees.J48 -- -C 0.001` become this:

```xml
 <option name="W" type="hyphens">
    <options type="classifier" value="weka.classifiers.trees.J48">
       <option name="C">0.001</option>
    </options>
 </option>
```
>  Internally, all the options enclosed by the `options` tag are pushed to the end after the `--` if one transforms the XML into a command line string.

* **quotes**
> A Meta-Classifier like `Stacking` can take several `-B` options, where each single one encloses other options in quotes (this itself can contain a Meta-Classifier!). From `-B "weka.classifiers.trees.J48"` we then get this XML:

```xml
 <option name="B" type="quotes">
    <options type="classifier" value="weka.classifiers.trees.J48"/>
 </option>
```
>  With the XML representation one doesn't have to worry anymore about the level of quotes one is using and therefore doesn't have to care about the correct escaping (i.e. " ... \" ... \" ...") since this is done automatically.

And if we now put all together we can transform this more complicated command line (`java` and the CLASSPATH omitted):

```
weka.classifiers.meta.Stacking -B "weka.classifiers.meta.AdaBoostM1 -W weka.classifiers.trees.J48 -- -C 0.001" -B "weka.classifiers.meta.Bagging -W weka.classifiers.meta.AdaBoostM1 -- -W weka.classifiers.trees.J48" -B "weka.classifiers.meta.Stacking -B \"weka.classifiers.trees.J48\"" -t test/datasets/hepatitis.arff
```

into XML:

```xml
 <options type="class" value="weka.classifiers.meta.Stacking">
    <option name="B" type="quotes">
       <options type="classifier" value="weka.classifiers.meta.AdaBoostM1">
          <option name="W" type="hyphens">
             <options type="classifier" value="weka.classifiers.trees.J48">
                <option name="C">0.001</option>
             </options>
          </option>
       </options>
    </option>

    <option name="B" type="quotes">
       <options type="classifier" value="weka.classifiers.meta.Bagging">
          <option name="W" type="hyphens">
             <options type="classifier" value="weka.classifiers.meta.AdaBoostM1">
                <option name="W" type="hyphens">
                   <options type="classifier" value="weka.classifiers.trees.J48"/>
                </option>
             </options>
          </option>
       </options>
    </option>

    <option name="B" type="quotes">
       <options type="classifier" value="weka.classifiers.meta.Stacking">
          <option name="B" type="quotes">
             <options type="classifier" value="weka.classifiers.trees.J48"/>
          </option>
       </options>
    </option>

    <option name="t">test/datasets/hepatitis.arff</option>
 </options>
```

**Note:**
> The `type` and `value` attribute of the outermost `options` tag is not used while reading the parameters. It is merely for documentation purposes, so that one knows which class was actually started from the command line.

**Responsible Class(es):**
`weka.core.xml.XMLOptions`

**Example(s):** [commandline.xml](files/commandline.xml)

# Serialization of Experiments
It is now possible to serialize the Experiments from the *WEKA Experimenter* not only in the proprietary binary format Java offers with serialization (with this you run into problems trying to read old experiments with a newer WEKA version, due to different SerialUIDs), but also in XML. There are currently two different ways to do this:


* **built-in**
>  The built-in serialization captures only the necessary informations of an experiment and doesn't serialize anything else. It's sole purpose is to save the setup of a specific experiment and can therefore not store any built models. Thanks to this limitation we'll never run into problems with mismatching SerialUIDs.

>  This kind of serialization is always available and can be selected via a Filter (*.xml) in the Save/Open-Dialog of the Experimenter.

>  The DTD is very simple and looks like this (for version 3.4.5):

```xml
 <!DOCTYPE object[
    <!ELEMENT object (#PCDATA | object)*>
    <!ATTLIST object name      CDATA #REQUIRED>
    <!ATTLIST object class     CDATA #REQUIRED>
    <!ATTLIST object primitive CDATA "no">
    <!ATTLIST object array     CDATA "no">   <!-- the dimensions of the array; no=0, yes=1 -->
    <!ATTLIST object null      CDATA "no">   
    <!ATTLIST object version   CDATA "3.4.5">
 ]>
```
>  Prior to versions 3.4.5 and 3.5.0 it looked like this:

```xml
 <!DOCTYPE object
 [
    <!ELEMENT object (#PCDATA | object)*>
    <!ATTLIST object name      CDATA #REQUIRED>
    <!ATTLIST object class     CDATA #REQUIRED>
    <!ATTLIST object primitive CDATA "yes">
    <!ATTLIST object array     CDATA "no">
 ]
 >
```
>  **Responsible Class(es):**
>  `weka.experiment.xml.XMLExperiment`

>  **for general Serialization:**
>  `weka.core.xml.XMLSerialization`
>  `weka.core.xml.XMLBasicSerialization`

>  **Example(s):** [serialization.xml](files/serialization.xml)

* **[KOML](http://old.koalateam.com/xml/serialization/)**
>  The Koala Object Markup Language (KOML) is published under the [LGPL](http://www.gnu.org/copyleft/lgpl.html) and is an alternative way of serializing and derserializing Java Objects in an XML file. Like the normal serialization it serializes everything into XML via an ObjectOutputStream, including the SerialUID of each class. Even though we have the same problems with mismatching SerialUIDs it is at least possible edit the XML files by hand and replace the offending IDs with the new ones.

>  In order to use KOML one only has to assure that the KOML classes are in the CLASSPATH with which the Experimenter is launched. As soon as KOML is present another Filter (*.koml) will show up in the Save/Open-Dialog.

>  The DTD for KOML can be found [here](http://old.koalateam.com/xml/koml12.dtd).

>  **Responsible Class(es):**
>  `weka.core.xml.KOML`

>  **Example(s):** [serialization.koml](files/serialization.koml)

The experiment class can of course read those XML files if passed as input or output file (see options of `weka.experiment.Experiment` and `weka.experiment.RemoteExperiment`).

# Serialization of Classifiers
The options for models of a classifier, `-l` for the input model and `-d` for the output model, now also supports XML serialized files. Here we have to differentiate between two different formats:


* **built-in**
> The built-in serialization captures only the options of a classifier but not the built model. With the `-l` one still has to provide a training file, since we only retrieve the options from the XML file. It is possible to add more options on the command line, but it is no check performed whether they collide with the ones stored in the XML file.
> The file is expected to end with `.xml`.

* **[KOML](http://old.koalateam.com/xml/serialization/)**
> Since the KOML serialization captures everything of a Java Object we can use it just like the normal Java serialization.
> The file is expected to end with `.koml`.

The **built-in** serialization can be used in the **Experimenter** for loading/saving options from algorithms that have been added to a Simple Experiment. Unfortunately it is not possible to create such a hierarchical structure like mentioned in [Command Line](xml.md#command-line). This is because of the loss of information caused by the `getOptions()` method of classifiers: 
it returns only a flat String-Array and not a tree structure.

**Responsible Class(es):**
`weka.core.xml.KOML`
`weka.classifiers.xml.XMLClassifier`

**Example(s):** [commandline_inputmodel.xml](files/commandline_inputmodel.xml)

# Bayesian Networks
The GraphVisualizer (`weka.gui.graphvisualizer.GraphVisualizer`) can save graphs into the [Interchange Format](http://sites.poli.usp.br/p/fabio.cozman/Research/InterchangeFormat/index.html) for Bayesian Networks (BIF). If started from command line with an XML filename as first parameter and not from the Explorer it can display the given file directly.

The DTD for BIF is this:

```xml
 <!DOCTYPE BIF [
    <!ELEMENT BIF ( NETWORK )*>
          <!ATTLIST BIF VERSION CDATA #REQUIRED>
    <!ELEMENT NETWORK ( NAME, ( PROPERTY | VARIABLE | DEFINITION )* )>
    <!ELEMENT NAME (#PCDATA)>

    <!ELEMENT VARIABLE ( NAME, ( OUTCOME |  PROPERTY )* ) >
          <!ATTLIST VARIABLE TYPE (nature|decision|utility) "nature">
    <!ELEMENT OUTCOME (#PCDATA)>
    <!ELEMENT DEFINITION ( FOR | GIVEN | TABLE | PROPERTY )* >
    <!ELEMENT FOR (#PCDATA)>
    <!ELEMENT GIVEN (#PCDATA)>

    <!ELEMENT TABLE (#PCDATA)>
    <!ELEMENT PROPERTY (#PCDATA)>
 ]>
```

**Responsible Class(es):**
`weka.classifiers.bayes.BayesNet#toXMLBIF03()`
`weka.classifiers.bayes.net.BIFReader`
`weka.gui.graphvisualizer.BIFParser`

**Example(s):** [bif.xml](files/bif.xml)

# Tools
* **Experimenter options**
> The XSLT script [options.xsl](files/options.xsl) parses an XML file for the experimenter and outputs the options in two ways:

	* in an array-like fashion, i.e., each option on a separate line; the class is output first.
	* commandline-like, i.e., the class followed by all its parameters; at each end of a line a "\" is appended. (works only on *nix and [Cygwin](http://www.cygwin.com/))
>  (Use [options_single.xsl](files/options_single.xsl)
>  **Usage:**
```bash
 xsltproc options.xsl <xml file>
```
>  *Note: 
you can use any XSLT processor, e.g., xt; xsltproc is just one.*

# Downloads
* KOML
	* [koml12.dtd](files/koml12.dtd) - local copy of the KOML DTD 1.2
	* [koml_bin.zip](files/koml_bin.zip)
	* [koml_sources.zip](files/koml_sources.zip) - the KOML source code 
