# Console
With command redirection one can redirect [standard streams](http://en.wikipedia.org/wiki/Standard_streams) like *stdin*, *stdout* and *stderr* to user-specified locations. Quite often it is useful to redirect the output of a program to a text file.

* redirecting *stdout* to a file

    ```
    someProgram >/some/where/output.txt     (Linux/Unix Bash)
    someProgram >c:\some\where\output.txt   (Windows command prompt)
    ```

* redirecting *stderr* to a file

    ```
    someProgram 2>/some/where/output.txt     (Linux/Unix Bash)
    someProgram 2>c:\some\where\output.txt   (Windows command prompt)
    ```

* redirecting *stdout* and *stderr* to a file

    ```
    someProgram &>/some/where/output.txt         (Linux/Unix Bash)
    someProgram >c:\some\where\output.txt 2>&1   (Windows command prompt)
    ```

**Note:** under Weka quite often the output is printed to *stderr*, e.g., if one is using the *-p 0* option from the commandline to print the predicted values for a test file:

```
 java weka.classifiers.trees.J48 -t train.arff -T test.arff -p 0 2> j48.txt
```

or if one already has a trained model:

```
 java weka.classifiers.trees.J48 -l j48.model -T test.arff -p 0 2> j48.txt
```

# SimpleCLI
One can perform a basic redirection also in the SimpleCLI, e.g.:

```
 java weka.classifiers.trees.J48 -t test.arff > j48.txt
```

**Note:** the **>** must be preceded and followed by a *space*, otherwise it is not recognized as redirection, but part of another parameter.

# Links
* Linux

    * [Command redirection under Bash](http://www.tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-3.html)
    * [I/O Redirection under Bash](http://www.tldp.org/LDP/abs/html/io-redirection.html)
    * [Redirection under Unix (WikiPedia)](http://en.wikipedia.org/wiki/Redirection_%28Unix%29)

* Windows

    * [Command redirection under MS Windows](http://technet.microsoft.com/en-us/library/bb490982.aspx)
    * [Command redirection under MS DOS](http://www.robvanderwoude.com/redirection.php)
