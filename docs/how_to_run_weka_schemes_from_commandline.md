It is quite often the case that one has to run a classifier, filter, attribute selection, etc. from commandline, leaving the comfort of the GUI (most likely the Explorer). Due to the vast amount of options the Weka schemes offer, it can be quite tedious setting up a scheme on the commandline.

In the following, a few different approaches are listed that can be used for running a scheme from the commandline:

* **Hardcore approach** (works for all versions of Weka)
> one just uses the `-h` option to display the commandline help with all available options and chooses the ones that apply, e.g.:
>> `java weka.classifiers.functions.SMO -h`
> The drawback of this method is, that one has to take care of escaping nested quotes oneself. As soon as one has to use meta-classifiers, this gets real messy. An introduction to the commandline use can be found in the [Primer](primer.md).

* **copy/paste approach**
> With this approach, one doesn't have to worry about correct nesting, since Weka takes care of that, returning correctly nested and escaped options.
* Since version 3.5.3, one can right-click (or `<Alt>+<Shift>` left-click for Mac users) any **GenericObjectEditor** panel and select the *Copy configuration to clipboard* option to copy the currently shown configuration to the clipboard and then just paste it into the commandline. One only needs to add the appropriate `java` call and other general options, like datasets, class index, etc.
* Another copy/paste approach is copying the configurations from the **Explorer** log, which is available since version 3.5.4. Every action in the Explorer, like applying a filter, running a classifier, attribute selection, etc. outputs the command to the log as well. This makes is fairly easy copying it to the clipboard and using it in the console, only the `java` call and other general options need to be added.

## See also
* [Primer](primer.md) - introduction to Weka from the commandline
* [CLASSPATH](classpath.md) - how to load all necessary libraries or welcome to the [JAR hell](https://en.wikipedia.org/wiki/Java_Classloader#JAR_hell)
* [Command redirection](command_redirection.md) - shows how to redirect output in files
