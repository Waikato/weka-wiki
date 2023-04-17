Articles such as [How do I use WEKA's classes in my own code?](../faqs/how_do_i_use_wekas_classes_in_my_own_code.md) and [How do I write a new classifier or filter?](../faqs/how_do_i_write_a_new_classifier_or_filter.md) describe how to extend Weka to add your own learning algorithms and so forth. This article describes how such enhancements can be assembled into a *package* that can be accessed via Weka’s [package management](../packages/manager.md) system. Bundling your enhancements in a package makes it easy to share with other Weka users.

In this article we refer to a *package* as an archive containing various resources such as compiled code, source code, javadocs, package description files (meta data), third-party libraries and configuration property files. Not all of the preceding may be in a given package, and there may be other resources included as well. This concept of a *package* is quite different to that of a Java packages, which simply define how classes are arranged hierarchically.

 
# Where does WEKA store packages and other configuration stuff?
By default, Weka stores packages and other information in `$WEKA_HOME`. The default location for `WEKA_HOME` is `user.home/wekafiles`, where `user.home` is the user’s home directory. You can change the default location for `WEKA_HOME` by setting this either as an evironment variable for your platform, or by specifying it as a Java property when starting Weka. E.g.:

```
export WEKA_HOME=/home/somewhere/weka_bits_and_bobs
```

will set the directory that Weka uses to `/home/somewhere/weka_bits_and_bobs` under the LINUX operating system.

The same thing can be accomplished when starting Weka by specifying a Java property on the command line, E.g.:

```
java -DWEKA_HOME=/home/somewhere/weka_bits_and_bobs -jar weka.jar
```

Inside `$WEKA_HOME` you will find the main weka log file (weka.log) and a number of directories:

* **packages** holds installed packages. Each package is contained its own subdirectory.
* **props** holds various Java property files used by Weka. This directory replaces the user’s home directory (used in earlier releases of Weka) as one of the locations checked by Weka for properties files (such as `DatabaseUtils.props`). Weka first checks, in order, the current directory (i.e. the directory that Weka is launched from), then `$WEKA_HOME/props` and finally the `weka.jar` file for property files.
* **repCache** holds the cached copy of the meta data from the central package repository. If the contents of this directory get corrupted it can be safely deleted and Weka will recreate it on the next restart.
* **systemDialogs** holds marker files that are created when you check *Don’t show this again* in various system popup dialogs. Removing this directory or its contents will cause Weka to display those prompts anew.

# Anatomy of a package
A Weka package is a zip archive that must unpack to the current directory. For example, the `DTNB` package contains the decision table naive Bayes hybrid classifier and is delivered in a file called `DTNB.zip`. When unpacked this zip file creates the following directory structure:

```
   <current directory>
     +-DTNB.jar
     +-Description.props
     +-build_package.xml
     +-src
     |   +-main
     |   |   +-java
     |   |       +-weka
     |   |           +-classifiers
     |   |               +-rules
     |   |                   +-DTNB.java
     |   +-test
     |       +-java
     |           +-weka
     |               +-classifiers
     |                   +-rules
     |                       +-DTNBTest.java
     +-lib
     +-doc
```

When installing, the package manager will use the value of the "PackageName" field in the Description.props file (see below) to create a directory in `$WEKA_HOME/packages` to hold the package contents. The contents of the `doc` directory have not been shown in the diagram above, but this directory contains javadoc for the `DTNB` class. A package must have a `Description.props` file and contain at least one jar file with compiled Java classes. The package manager will attempt to load all jar files that it finds in the root directory and the `lib` directory. Other files are optional, but if the package is open-source then it is nice to include the source code and an ant build file that can be used to compile the code. Template versions of the [Description.props](../files/Description.props) file and
[build_package.xml](../files/build_package.xml) file are available from the Weka site and here.

# The description file
A valid package must contain a `Description.props` file that provides meta data on the package. Identical files are stored at the central package repository and the local cache maintained by the package manager. The package manager uses these files to compare what is installed to what is available and resolve dependencies.

The [Description.props](../files/Description.props) contains basic information on the package in the following format:

```
# Template Description file for a Weka package
 
# Package name (required)
PackageName=funkyPackage
 
# Version (required)
Version=1.0.0
 
#Date (year-month-day)
Date=2010-01-01
 
# Title (required)
Title=My cool algorithm
 
# Category (recommended)
Category=Classification
 
# Author (required)
Author=Joe Dev <joe@somewhere.net>,Dev2 <dev2@somewhereelse.net>
 
# Maintainer (required)
Maintainer=Joe Dev <joe@somewhere.net>
 
# License (required)
License=GPL 2.0|Mozilla
 
# Description (required)
Description=This package contains the famous Funky Classifer that performs \
 truely funky prediction.
 
# Changes and/or bug fixes in this package (optional)
Changes=Fixed a serious bug that affected overall coolness of the Funky Classifier
 
# Package URL for obtaining the package archive (required)
PackageURL=http://somewhere.net/weka/funkyPackage.zip
 
# URL for further information
URL=http://somewhere.net/funkyResearchInfo.html
 
# Enhances various other packages?
Enhances=packageName1,packageName2,...
 
# Related to other packages?
Related=packageName1,packageName2,...
 
# Dependencies (required; format: packageName (equality/inequality version_number)
Depends=weka (>=3.7.1), packageName1 (=x.y.z), packageName2 (>u.v.w|<=x.y.z),...
```

The *PackageName* and *Version* give the name of the package and version number respectively. The name can consist of letters, numbers, and the dot character. It should not start with a dot and should not contain any spaces. The version number is a sequence of three non-negative integers separated by single *.* or *-* characters.

The *Title* field should give a one sentence description of the package. The *Description* field can give a longer description of the package spaning multiple sentences. It may include technical references and can use HTML markup.

The *Category* field is strongly recommended as this information is displayed on both the repository web site and in the GUI package manager client. In the latter, the user can sort the packages on the basis of the category field. It is recommended that an existing category be assigned if possible. Some examples include (Classification, Text classification, Ensemble learning, Regression, Clustering, Associations, Preprocessing, Visualization, Explorer, Experimenter, KnowledgeFlow).

The *Author* field describes who wrote the package and may include multiple names (separated by commas). Email addresses may be given in angle brackets after each name. The field is intended for human readers and no email addresses are automatically extracted.

The *Maintainer* field lists who maintains the package and should include a single email address, enclosed in angle brackets, for sending bug reports to.

The *License* field lists the license(s) that apply to the package. This field may contain the short specification of a license (such as LGPL, GPL 2.0 etc.) or the string *file LICENSE*, where *LICENSE* exists as a file in the top-level directory of the package. The string *Unlimited* may be supplied to indicate that there are no restrictions on distribution or use aside from those imposed by relevant laws.

The *PackageURL* field lists valid URL that points to the package zip file. This URL is used by the package manager to download and install the package.

The required *Depends* field gives a comma separated list of packages which this package depends on. The name of a package is followed by a version number constraint enclosed in parenthesis. Valid operators for version number constraints include =, <, >, <=, >=. The keyword *weka* is reserved to refer to the base Weka system and can be used to indicate a dependency on a particular version of Weka. At a minimum, the *Depends* field should list the base version of Weka that the package will operate with. Some examples include:

``` 
Depends=weka (>=3.7.2), DTNB (=1.0.0)
```

states that this package requires Weka 3.7.2 or higher and version 1.0.0 of the package DTNB.

``` 
Depends=weka (>3.7.1|<3.8.0)
```

states that this package requires a version of Weka between 3.7.1 and 3.8.0.

``` 
Depends=weka (>=3.7.2), DTNB (<1.5.0|>=2.0.1)
```

states that this package requires that a version of the DTNB package be installed that is either less than version 1.5.0 or greater than or equal to version 2.0.1.

If there is no version number constraint following a package name, the package manager assumes that the latest version of the dependent package is suitable.

The optional *URL* field gives a URL at which the user can find additional online information about the package or its constituent algorithms.

The optional *Enhances* field can be used to indicate which other packages this package is based on (i.e. if it extends methods/algorithms from another<br />  package in some fashion).

The optional *Related* field is similar to the *Enhances* field. It can be used to point the user to other packages that are related in some fashion to this one.

The optional *Changes* field should be used to indicate what changes/bug fixes are included in the current release of the package.

There are several other fields that can be used to provide information to assist the user with completing installation (if it can’t be completely accomplished with the package zip file) or display error messages if necessary components are missing:

```
MessageToDisplayOnInstallation=Funky package requires some extra\n\
 stuff to be installed after installing this package. You will\n\
 need to blah, blah, blah in order to blah, blah, blah...
 
DoNotLoadIfFileNotPresent=lib/someLibrary.jar,otherStuff/important,...
 
DoNotLoadIfFileNotPresentMessage=funkyPackage can't be loaded because some \
 funky libraries are missing. Please download funkyLibrary.jar from \
 http://www.funky.com and install in $WEKA_HOME/packages/funkyPackage/lib
 
DoNotLoadIfClassNotPresent=com.some.class.from.some.Where,org.some.other.Class,...
 
DoNotLoadIfClassNotPresentMessage=funkyPackage can't be loaded because \
 com.funky.FunkyClass can't be instantiated. Have you downloaded and run \
 the funky software installer for your platform?
```

The optional *MessageToDisplayOnInstallation* field allows you to specify special instructions to the user in order to help them complete the intallation manually. This message gets displayed on the console, written to the log and appears in a pop-up information dialog if using the GUI package manager. It should include *\n* in order to avoid long lines when displayed in a GUI pop-up dialog.

The optional *DoNotLoadIfFileNotPresent* field can be used to prevent Weka from loading the package if the named ''files'' and/or ''directories'' are not present in the package’s installation directory. An example is the massiveOnlineAnalysis package. This package is a connector only package and does not include the MOA library. Users of this package must download the moa.jar file separately and copy it to the package’s `lib` directory manually. Multiple files and directories can be specified as a comma separated list. All paths are relative to the top-level directory of the package. **IMPORTANT**: use forward slashes as separator characters, as these are portable accross all platforms. The *DoNotLoadIfFileNotPresentMessage* field can be used to supply an optional message to display to the user if Weka detects that a file or directory is missing from the package. This message will be displayed on the console and in the log.

The optional *DoNotLoadIfClassNotPresent* field can be used to prevent Weka from loading the package if the named ''class(es)'' can’t be instantiated. This is useful for packages that rely on stuff that has to be installed manually by the user. For example, Java3D is a separate download on all platforms except for OSX, and installs itself into the system JRE/JDK. The *DoNotLoadIfClassNotPresentMessage* field can be used to supply an optional message to display to the user if Weka detects that a class can’t be instantiated. Again, this will be displayed on the console and in the log.

New in Weka 3.9.2 and 3.8.2 is the ability to constrain the OS and architecture that a package can be installed and loaded on. Two new fields are used for this:

```
# Specify which OS's the package can operate with. Omitting this entry indicates no restrictions on OS. (optional)
OSName=Windows,Mac,Linux
 
# Specify which architecture the package can operate with. Omitting this entry indicates no restriction. (optional)
OSArch=64
```

Entries in the OSName field are compared against the value of the Java property "os.name" using a String.toLowerCase().contains() operation. Any single match indicates a pass. If an OSName field exists in the Description.props, and it matches, then the optional OSArch field is examined. If present, values in the OSArch list are compared against the value of the Java property "os.arch". The special OSArch entries "32" and "64" are tested using a String().contains() operation; all other entries are compared using String.equalsIgnoreCase().

 
## Additional configuration files
Certain types of packages may require additional configuration files to be present as part of the package. The last chapter covered various ways in which Weka can be extended without having to alter the core Weka code. These plugin mechanisms have been subsumed by the package management system, so some of the configuration property files they require must be present in the package’s top-level directory if the package in question contains such a plugin. Examples include additional tabs for the Explorer, mappings to custom property editors for Weka’s GenericObjectEditor and Knowledge Flow plugins. Here are some examples:

The scatterPlot3D package adds a new tab to the Explorer. In order to accomplish this a property has to be set in the Explorer.props file (which contains default values for the Explorer) in order to tell Weka to instantiate and display the new panel. The scatterPlot3D file includes an *Explorer.props* file in its top-level directory that has the following contents:

```
# Explorer.props file. Adds the Explorer3DPanel to the Tabs key.
Tabs=weka.gui.explorer.Explorer3DPanel
TabsPolicy=append
```

This property file is read by the package management system when the package is loaded and any key-value pairs are added to existing Explorer properties that have been loaded by the system at startup. If the key already exists in the Explorer properties, then the package has the option to either replace (i.e. overwrite) or append to the existing value. This can be specified with the "TabsPolicy" key. In this case, the value `weka.gui.explorer.Explorer3DPanel` is appended to any existing value associated with the "Tabs" key. `Explorer3DPanel` gets instantiated and added as a new tab when the Explorer starts.

Another example is the kfGroovy package. This package adds a plugin component to Weka’s Knowledge Flow that allows a Knowledge Flow step to be implemented and compiled dynamically at runtime as a Groovy script. In order for the Knowledge Flow to make the new step appear in its *Plugins* toolbar, there needs to be a *Beans.props* file in the package’s top level directory. In the case of kfGroovy, this property file has the following contents:

```
# Specifies that this component goes into the Plugins toolbar
weka.gui.beans.KnowledgeFlow.Plugins=org.pentaho.dm.kf.GroovyComponent
```

The new pluggable evaluation metrics for classification/regression (from Weka 3.7.8 and nightly developer snapshots from 15-11-2012) are managed by the PluginManager class. To tell PluginManager that your package provides a new evaluation metric you need to provide a "PluginManager.props" file in the package's top level directory. For example, a hypothetical bobsMetric package might declare a new "Area under Bob curve" metric like so:

```
# Specify a new plugin Evaluation metric
weka.classifiers.evaluation.AbstractEvaluationMetric=weka.classifiers.evaluation.BobsAUC
```

# Contributing a package
If you have created a package for Weka then there are two options for making it available to the community. In both cases, hosting the package’s zip archive is the responsibility of the contributer.

The first, and official, route is to contact the current Weka maintainer (normally also the admin of the WEKA homepage) and supply your package’s `Description.props` file. The Weka team will then test downloading and using your package to make sure that there are no obvious problems with what has been specified in the `Description.props` file and that the software runs and does not contain any malware/malicious code. If all is well, then the package will become an *official* Weka package and the central package repository meta data will be updated with the package’s `Description.props file`. 

**Responsibility for maintaining and supporting the package resides with the contributer**.

The second, and unofficial, route is to simply make the package’s zip archive available on the web somewhere and advertise it yourself. Although users will not be able to browse it’s description in the official package repository, they will be able to download and install it directly from your URL by using the command line version of the package manager. This route could be attractive for people who have published a new algorithm and want to quiclky make a beta version available for others to try without having to go through the official route.

 
# Creating a mirror of the package meta data repository
In this section we discuss an easy approach to setting up and maintaining a mirror of the package meta data repository. Having a local mirror may provide faster access times than to that of the official repository on Sourceforge. Extending this approach to the creation of an alternative central repository (hosting packages not available at the official repository) should be straight forward.

Just about everything necessary for creating a mirror exists in the local meta data cache created by Weka’s package management system. This cache resides at `$WEKA_HOME/repCache`. The only thing missing (in Weka 3.7.2) for a complete mirror is the file *images.txt*, that lists all the image files used in the html index files. This file contains the following two lines:

``` 
Title-Bird-Header.gif
pentaho_logo_rgb_sm.png
```

*images.txt* is downloaded automatically by the package management system in Weka 3.7.3 and higher.

To create a mirror:
1. Copy the contents of `$WEKA_HOME/repCache` to a temporary directory. For the purposes of this example we’ll call it `tempRep`
2. Change directory into `tempRep` and run `java weka.core.RepositoryIndexGenerator .`. Don't forget the "." after the command (this tells `RepoistoryIndexGenerator` to operate on the current directory)
3. Change directory to the parent of `tempRep` and synchronize its contents to wherever your web server is located (this is easy via rsync under Nix-like operating systems).

`RepositoryIndexGenerator` automatically creates the main index.html file, all the package index.html files and html files correpsonding to all version prop files for each package. It will also create `packageList.txt` and `numPackages.txt` files.

**IMPORTANT**: Make sure that all the files in `tempRep` are world readable. It is easy to make packages available that are not part of the official Weka repository. Assuming you want to add a package called *funkyPackage* (as specified by the *PackageName* field in the `Description.props` file):

1. Create a directory called *funkyPackage* in `tempRep`
2. Copy the `Description.props` file to `tempRep/funkyPackage/Latest.props`
3. Copy the `Description.props file` to `tempRep/funkyPackage/<version number>.props`, where *version number* is the version number specified in the *Version* field of `Description.props`
4. Run `RepositoryIndexGenerator` as described previously and sync `tempRep` to your web server

Adding a new version of an existing package is very similar to what has already been described. All that is required is that the new `Description.props` file corresponding to the new version is copied to `Latest.props` and to `<version numer>.props` in the package’s folder. Running `RepositoryIndexGenerator` will ensure that all necessary html files are created and supporting text files are updated.

Automating the mirroring process would simply involve using your OS’s scheduler to execute a script that:

1. Runs `weka.core.WekaPackageManager -refresh-cache`
2. rsyncs `$WEKA_HOME/repCache` to `tempRep`
3. Runs `weka.core.RepoistoryIndexGenerator`
4. rsyncs `tempRep` to your web server

