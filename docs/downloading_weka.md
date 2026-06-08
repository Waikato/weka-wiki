
There are two versions of Weka: Weka 3.8 is the latest stable version
and Weka 3.9 is the development version. New releases of these two versions
are normally made once or twice a year. 

The stable version receives only bug fixes and feature upgrades that
do not break compatibility with its earlier releases, while the
development version may receive new features that break compatibility
with its earlier releases.

Weka 3.8 and 3.9 feature a package management system that makes it
easy for the Weka community to add new functionality to Weka. The
package management system requires an internet connection in order to
download and install packages.

# Stable version

Weka 3.8 is the latest stable version of Weka. This branch of Weka
only receives bug fixes and upgrades that do not break compatibility
with earlier 3.8 releases, although major new features may become
available in packages.  There are different options for downloading
and installing it on your system:

### Windows - Intel processors

* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-8-7-bellsoft-x64-windows.exe)
to download a self-extracting executable for 64-bit Windows that
includes Bellsoft's 64-bit OpenJDK Java VM 25 for Intel Windows
(weka-3-8-7-bellsoft-x64-windows.exe; 175.7 MB)

This executable will install Weka in your Program Menu. Launching via the Program
Menu or shortcuts will automatically use the included JVM to run Weka.

### Windows - ARM processors

* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-8-7-bellsoft-arm-windows.exe)
to download a self-extracting executable for 64-bit Windows that
includes Bellsoft's 64-bit OpenJDK Java VM 25 for ARM Windows
(weka-3-8-7-bellsoft-arm-windows.exe; 105.5 MB)

This executable will install Weka in your Program Menu. Launching via the Program
Menu or shortcuts will automatically use the included JVM to run Weka.

### Mac OS - Intel processors
                  
* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-8-7-bellsoft-x64-osx.dmg)
to download a disk image for Mac OS that contains a
Mac application including Bellsoft's 64-bit OpenJDK Java VM 25 for Intel Macs.
(weka-3-8-7-bellsoft-x64-osx.dmg; 238.5 MB)

### Mac OS - ARM processors
                  
* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-8-7-bellsoft-arm-osx.dmg)
to download a disk image for Mac OS that contains a
Mac application including Bellsoft's 64-bit OpenJDK Java VM 25 for ARM Macs.
(weka-3-8-7-bellsoft-arm-osx.dmg; 231.8 MB)

### Linux - Intel processors

* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-8-7-bellsoft-x64-linux.zip)
to download a zip archive for Linux that includes Bellsoft's 64-bit OpenJDK Java VM 25
for X86 Linux (weka-3-8-7-bellsoft-x64-linux.zip; 200.3 MB)

First unzip the zip file. This will create a new directory called
weka-3-8-7. To run Weka, change into that directory and type

``` bash
./weka.sh
```

### Linux - ARM processors

* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-8-7-bellsoft-arm-linux.zip)
to download a zip archive for Linux that includes Bellsoft's 64-bit OpenJDK Java VM 25
for ARM Linux (weka-3-8-7-bellsoft-arm-linux.zip; 203.5 MB)

First unzip the zip file. This will create a new directory called
weka-3-8-7. To run Weka, change into that directory and type

``` bash
./weka.sh
```


### Other platforms

* Click [here](https://prdownloads.sourceforge.net/weka/weka-3-8-7.zip)
 to download a zip archive containing Weka (weka-3-8-7.zip; 57.7 MB)

First unzip the zip file. This will create a new directory called
weka-3-8-7. To run Weka, change into that directory and type

``` bash
java -jar weka.jar
```

Note that Java needs to be installed on your system for this to
work. Also note that using `-jar` will override your current
CLASSPATH variable and only use the `weka.jar`.

# Developer version

This is the main development trunk of Weka and continues from the stable Weka 3.8 code line. It
may receive new features that break backwards compatibility.

### Windows - Intel processors

* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-9-7-bellsoft-x64-windows.exe)
to download a self-extracting executable for 64-bit Windows that
includes Bellsoft's 64-bit OpenJDK Java VM 25 for Intel Windows
(weka-3-9-7-bellsoft-x64-windows.exe; 175.7 MB)

This executable will install Weka in your Program Menu. Launching via the Program
Menu or shortcuts will automatically use the included JVM to run Weka.

### Windows - ARM processors

* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-9-7-bellsoft-arm-windows.exe)
to download a self-extracting executable for 64-bit Windows that
includes Bellsoft's 64-bit OpenJDK Java VM 25 for ARM Windows
(weka-3-9-7-bellsoft-arm-windows.exe; 105.5 MB)

This executable will install Weka in your Program Menu. Launching via the Program
Menu or shortcuts will automatically use the included JVM to run Weka.

### Mac OS - Intel processors
                  
* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-9-7-bellsoft-x64-osx.dmg)
to download a disk image for Mac OS that contains a
Mac application including Bellsoft's 64-bit OpenJDK Java VM 25 for Intel Macs.
(weka-3-9-7-bellsoft-x64-osx.dmg; 238.5 MB)

### Mac OS - ARM processors
                  
* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-9-7-bellsoft-arm-osx.dmg)
to download a disk image for Mac OS that contains a
Mac application including Bellsoft's 64-bit OpenJDK Java VM 25 for ARM Macs.
(weka-3-9-7-bellsoft-arm-osx.dmg; 231.8 MB)

### Linux - Intel processors

* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-9-7-bellsoft-x64-linux.zip)
to download a zip archive for Linux that includes Bellsoft's 64-bit OpenJDK Java VM 25
for X86 Linux (weka-3-9-7-bellsoft-x64-linux.zip; 200.4 MB)

First unzip the zip file. This will create a new directory called
weka-3-9-7. To run Weka, change into that directory and type

``` bash
./weka.sh
```

### Linux - ARM processors

* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-9-7-bellsoft-arm-linux.zip)
to download a zip archive for Linux that includes Bellsoft's 64-bit OpenJDK Java VM 25
for ARM Linux (weka-3-9-7-bellsoft-arm-linux.zip; 203.5 MB)

First unzip the zip file. This will create a new directory called
weka-3-9-7. To run Weka, change into that directory and type

``` bash
./weka.sh
```


### Other platforms

* Click [here](https://prdownloads.sourceforge.net/weka/weka-3-9-7.zip)
 to download a zip archive containing Weka (weka-3-9-7.zip; 57.7 MB)

First unzip the zip file. This will create a new directory called
weka-3-9-7. To run Weka, change into that directory and type

``` bash
java -jar weka.jar
```

Note that Java needs to be installed on your system for this to
work. Also note that using `-jar` will override your current
CLASSPATH variable and only use the `weka.jar`.

# Old versions

All old versions of Weka are available from the [Sourceforge
website](https://sourceforge.net/projects/weka/).

### Upgrading from Weka 3.7

In case you are upgrading an existing Weka 3.7 installation, if the
Weka 3.8 package manager does not start up, please delete the file
`installedPackageCache.ser` in the `packages` folder that resides in
the `wekafiles` folder in your user home. Also, serialized Weka models
created in 3.7 are incompatible with 3.8. The [model
migrator](https://ml.cms.waikato.ac.nz/weka/modelMigrator.jar)
tool can migrate some models to 3.8 (a known
exception is RandomForest). Usage is as follows:

``` bash 
java -cp <path to modelMigrator.jar>:<path to weka.jar> weka.core.ModelMigrator -i <path to old serialized weka mode> -o <upgraded model file name>
```
