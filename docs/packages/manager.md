Usually, the term "package" is used to refer to Java's concept of organizing classes. From version 3.7.2, Weka has the concept of a package as a bundle of additional functionality, separate from that supplied in the main weka.jar file. A package consists of various jar files, documentation, meta data, and possibly source code. Many learning algorithms and tools that were present in earlier versions of Weka have become separate packages from version 3.7.2. This simplifies the core Weka system and allows users to install just what they need or are interested in. It also provides a simple mechanism for people to use when contributing to Weka. There are a number of packages available for Weka that add learning schemes or extend the functionality of the core system in some fashion. Many are provided by the Weka team and others are from third parties.

Weka includes a facility for the management of packages and a mechanism to load them dynamically at runtime. There is both a command-line and GUI package manager. If the package manager does not start when you try to run it, take a look at [this](../faqs/package_manager_doesnt_start.md) page.

# Command line package management
 Assuming that the weka.jar file is in the classpath, the package manager can be accessed by typing:

```
 java weka.core.WekaPackageManager
```

Supplying no options will print the usage information:

```
Usage: weka.core.WekaPackageManager [option]
Options:
  -list-packages <all | installed | available>
  -package-info <repository | installed | archive> packageName
  -install-package <packageName | packageZip | URL> [version]
  -uninstall-package <packageName>
  -refresh-cache
```

---

Weka 3.7.8 now offers a completely "offline" mode that involves no attempts to connect to the internet. This mode can be used to install package zip files that the user already has on the file system, and to browse already installed packages. This mode can be accessed from the command line package manager by specifying the "-offline" option. Alternatively, the property weka.packageManager.offline=true can be provided to the Java virtual machine on the command line or in a properties file (see the section on properties below).

----

Information (meta data) about packages is stored on a web server hosted on Sourceforge. The first time the package manager is run, for a new installation of Weka, there will be a short delay while the system downloads and stores a cache of the meta data from the server. Maintaining a cache speeds up the process of browsing the package information. From time to time you should update the local cache of package meta data in order to get the latest information on packages from the server. This can be achieved by supplying the `-refresh-cache` option.

  The `-list-packages` option will, as the name suggests, print information (version numbers and short descriptions) about various packages. The option must be followed by one of three keywords:

* `all` will print information on all packages that the system knows about
* `installed` will print information on all packages that are installed locally
* `available` will print information on all packages that are not installed

The following shows an example of listing all packages installed locally:

```
java weka.core.WekaPackageManager -list-packages installed
 
Installed    Repository    Package
=========    ==========    =======
1.0.0        1.0.0         DTNB: Class for building and using a decision table/naive bayes hybrid classifier.
1.0.0        1.0.0         massiveOnlineAnalysis: MOA (Massive On-line Analysis).
1.0.0        1.0.0         multiInstanceFilters: A collection of filters for manipulating multi-instance data.
1.0.0        1.0.0         naiveBayesTree: Class for generating a decision tree with naive Bayes classifiers at the leaves.
1.0.0        1.0.0         scatterPlot3D: A visualization component for displaying a 3D scatter plot of the data using Java 3D.
```

The `-package-info` command lists information about a package given its name. The command is followed by one of three keywords and then the name of a package:

* `repository` will print info from the repository for the named package
* `installed` will print info on the installed version of the named package
* `archive` will print info for a package stored in a zip archive. In this case, the “archive” keyword must be followed by the path to an package zip archive file rather than just the name of a package

The following shows an example of listing information for the “isotonicRegression” package from the server:

```
java weka.core.WekaPackageManager -package-info repository isotonicRegression
Description:Learns an isotonic regression model. Picks the attribute that results in the lowest squared error. Missing values are not allowed. Can only deal with numeric attributes. Considers the monotonically increasing case as well as the monotonically decreasing case.
Version:1.0.0
PackageURL:http://prdownloads.sourceforge.net/weka/isotonicRegression1.0.0.zip?download
Author:Eibe Frank
PackageName:isotonicRegression
Title:Learns an isotonic regression model.
Date:2009-09-10
URL:https://weka.sourceforge.io/doc.dev/weka/classifiers/IsotonicRegression.html
Category:Regression
Depends:weka (>=3.7.1)
License:GPL 2.0
Maintainer:Weka team <wekalist@list.scms.waikato.ac.nz>
```

The `-install-package` command allows a package to be installed from one of three locations:

* specifying a name of a package will install the package using the information in the package description meta data stored on the server. If no version number is given, then the latest available version of the package is installed.
* providing a path to a zip file will attempt to unpack and install the archive as a Weka package
* providing a URL (beginning with `http://`) to a package zip file on the web will download and attempt to install the zip file as a Weka package

The `uninstall-package` command will uninstall the named package. Of course, the named package has to be installed for this command to have any effect!

 
## Running installed learning algorithms
Running learning algorithms that come with the main weka distribution (i.e. are contained in the weka.jar file) was covered earlier in the [Primer](../primer.md). But what about algorithms from packages that you’ve installed using the package manager? We don’t want to have to add a ton of jar files to our classpath every time we wan’t to run a particular algorithm. Fortunately, we don’t have to. Weka has a mechanism to load installed packages dynamically at run time. This means that newly installed packages are available in Weka's GUIs immediately.

  What about running algorithms from packages on the command line I hear you ask? We can run a named algorithm by using the Run command:

``` 
 java weka.Run
```

If no arguments are supplied, then Run outputs the following usage information:

``` 
 Usage:
     weka.Run [-no-scan] [-no-load] <scheme name [scheme options]>
```

The Run command supports sub-string matching, so you can run a classifier (such as J48) like so:

``` 
 java weka.Run J48
```

When there are multiple matches on a supplied scheme name you will be presented with a list. For example:

``` 
 java weka.Run NaiveBayes
 
 Select a scheme to run, or <return> to exit:
     1) weka.classifiers.bayes.ComplementNaiveBayes
     2) weka.classifiers.bayes.NaiveBayes
     3) weka.classifiers.bayes.NaiveBayesMultinomial
     4) weka.classifiers.bayes.NaiveBayesMultinomialUpdateable
     5) weka.classifiers.bayes.NaiveBayesSimple
     6) weka.classifiers.bayes.NaiveBayesUpdateable
 
 Enter a number >
```

You can turn off the scanning of packages and sub-string matching by providing the `-no-scan` option. This is useful when using the `Run` command in a script. In this case, you need to specify the fully qualified name of the algorithm to use. E.g.

``` 
 java weka.Run -no-scan weka.classifiers.bayes.NaiveBayes
```

To reduce startup time you can also turn off the dynamic loading of installed packages by specifying the `-no-load` option. In this case, you will need to explicitly include any packaged algorithms in your classpath if you plan to use them. E.g.

```
java -classpath ./weka.jar:$HOME/wekafiles/packages/DTNB/DTNB.jar rweka.Run -no-load -no-scan weka.classifiers.rules.DTNB
```


# GUI package manager
As well as a command line client, there is also a graphical interface to Weka’s package management system. This is available from the `Tools` menu in the `GUIChooser`. All the functionality available in the command line client to the package management system is available in the GUI version, along with the ability to install and uninstall multiple packages in one hit.

![Screenshot](../img/PackageManagerMain.png)

 The package manager’s window is split horizontally into two parts: at the top is a list of packages and at the bottom is a mini browser that can be used to display information on the currently selected package.

The package list shows the name of a package, its category, the currently installed version (if installed), the latest version available via the repository and whether the package has been loaded or not. This list may be sorted by either package name or category by clicking on the appropriate column header. A second click on the same header reverses the sort order. Three radio buttons in the upper left of the window can be used to filter what is displayed in the list. All packages (default), all available packages (i.e. those not yet installed) or only installed packages can be displayed.

If multiple versions of a package are available, they can be accessed by clicking on an entry in the “Repository version” column:

![Screenshot](../img/PackageManagerVersions.png)

 
## Installing and removing packages
At the very top of the window are three buttons. On the left-hand side is a button that can be used to refresh the cached copy of the package repository meta data. The first time that the package manager (GUI or command line) is used there will be a short delay as the initial cache is established. 

**NOTE: Weka (3.7.2) will not automatically check for new information at the central repository, so it is a good idea to refresh the local cache regularly. From Weka 3.7.3 the package manager will notify you if there are new packages available at the central repository**.

The two buttons at the top right are used to install and remove packages repspectively. Multiple packages may be installed/removed by using a shift-left-click combination to select a range and/or by using a command-left-click combination to add to the selection. Underneath the install and uninstall but- tons is a checkbox that can be enabled to ignore any dependencies required by selected packages and any conflicts that may occur. Installing packages while this checkbox is selected will '''not''' install required dependencies.

Some packages may have additional information on how to complete the installation or special instructions that gets displayed when the package is installed:

![Screenshot](../img/PackageManagerSpecial.png)

Usually it is not necessary to restart Weka after packages have been installed—the changes should be available immediately. An exception is when upgrading a package that is already installed. If in doubt, restart Weka.

 
## Unofficial packages
The package list shows those packages that have their meta data stored in Weka’s central meta data repository. These packages are “official” Weka packages and the Weka team as verified that they appear to provide what is advertised (and do not contain malicious code or malware).

It is also possible to install an *unofficial* package that has not gone through the process of become official. Unofficial packages might be provided, for exam- ple, by researchers who want to make experimental algorithms quickly available to the community for feedback. Unofficial packages can be installed by clicking the “File/url” button on the top-right of the package manager window. This will bring up an “Unnoficial package install” dialog where the user can browse their file system for a package zip file or directly enter an URL to the package zip file. Note that no dependency checking is done for unofficial packages.
 
## Using a HTTP proxy
Both the GUI and command line package managers can operate via a http proxy. To do so, start Weka from the command line and supply property values for the proxy host and port:

``` 
 java -Dhttp.proxyHost=some.proxy.somewhere.net -Dhttp.proxyPort=port weka.gui.GUIChooser
```

If your proxy requires authentication, then Weka will present a GUI dialog where you can enter your username and password. If you are running on a headless environment, then two more (non-standard) properties can be supplied:

``` 
 -Dhttp.proxyUser=some_user_name -Dhttp.proxyPassword=some_password
```

## Using an alternative central package meta data repository
By default, both the command-line and GUI package managers use the central package meta data repository hosted on Sourceforge. In the unlikely event that this site is unavailable for one reason or another, it is possible to point the package management system at an alternative repository. This mechanism allows a temporary backup of the official repostory to be accessed, local mirrors to be established and alternative repositories to be set up for use etc.

An alternative repository can be specified by setting a Java property:

``` 
weka.core.wekaPackageRepositoryURL=http://some.mirror.somewhere
```

This can either be set when starting Weka from the command line with the `-D` flag, or it can be placed into a file called “PackageRepository.props” in `$WEKA_HOME/props`. The default value of `WEKA_HOME` is `user.home/wekafiles`, where `user.home` is the user’s home directory. More information on how and where Weka stores configuration information is given in the [Package Structure](structure.md) article.

 
# Package manager property file
As mentioned in the previous section, an alternative package meta data repository can be specified by placing an entry in the PackageRepository.props file in `$WEKA_HOME/props`. From Weka 3.7.8, the package manager also looks for properties placed in `$WEKA_HOME/props/PackageManager.props`. The current set of properties that can be set are:

```
weka.core.wekaPackageRepositoryURL=http://some.mirror.somewhere
weka.packageManager.offline=[true | false]
weka.packageManager.loadPackages=[true | false]
weka.pluginManager.disable=com.funky.FunkyExplorerPluginTab
```

The default for offline mode (if unspecified) is `false` and for loadPackages is `true`. The weka.pluginManager.disable property can be used to specify a comma-separated list of fully qualified class names to "disable" in the GUI. This can be used to make problematic components unavailable in the GUI without having to prevent the entire package that contains them from being loaded. E.g. "funkyPackage" might provide several classifiers and a special Explorer plugin tab for visualization. Suppose, for example, that the plugin Explorer tab has issues with certain data sets and causes annoying exceptions to be generated (or perhaps in the worst cases crashes the Explorer!). In this case we might want to use the classifiers provided by the package and just disable the Explorer plugin. Listing the fully qualified name of the Explorer plugin as a member of the comma-separated list associated with the weka.pluginManager.disable property will achieve this.

