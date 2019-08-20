
The following guide explains how to put Weka on a USB memory stick. This works both for Linux and Windows (Mac OSX as well).

For simplicity, this example is demonstrated with the following versions:

* Weka: 
3.5.3
* JRE: 
1.5.0_10

# Preliminaries
* [download](http://www.cs.waikato.ac.nz/~ml/weka/) a Weka ZIP (Windows: 
don't download the Installer!)
* [download](http://java.sun.com/) the JRE (Java Runtime Environemnt) that works with the downloaded Weka version. (Linux: 
don't download the *RPM*, but the *Linux self-extracting file*)
* install the downloaded JRE
	* Windows: 
> the JRE location is normally in `C:\Program Files\Java`
	* Linux: 
> the self-extracting file creates a directory containing the JRE at the same location as the installation file

# Setup
* create a directory on your memory stick that will hold Weka and the JRE:

```text
 weka
```
* unzip the Weka ZIP into the *weka* directory, which will create the following sub-directory;
```text
 weka-3-5-3
```
* copy the JRE onto the stick in the *weka* directory, which will be this sub-directory:

```text
 jre1.5.0_10
```

# Script
As a final step, create a script to start Weka:

* Windows
	* create a new batch file called `weka.bat` in the directory `weka` with the following content
```winbatch
 @echo off
 set CP=%CLASSPATH%;.\weka-3-5-3\weka.jar
 start .\jre1.5.0_10\bin\javaw -classpath "%CP%" weka.gui.GUIChooser
```
>  **Note:** If `start` is not available in your flavor of Windows, you can drop it. It is only used to get rid of the DOS-Box.

* Linux
	* create a new bash script called `weka.sh` in the directory `weka` with the following content
```bash
 #!/bin/bash
 CP=$CLASSPATH:./weka-3-5-3/weka.jar
 ./jre1.5.0_10/bin/java -classpath $CP weka.gui.GUIChooser
```
>  **Note:** since memory sticks normally use the [FAT32](http://en.wikipedia.org/wiki/fat32) file-system you probably won't need to make it executable

# Execution

* Windows: 
> just double-click on the batch file

* Linux: 
> open a terminal and execute the bash script

# Links
* [Weka homepage](http://www.cs.waikato.ac.nz/~ml/weka/)
* [Sun Java homepage](http://java.sun.com/)
* [start command](http://www.robvanderwoude.com/ntstart.html) in Windows NT/2000
