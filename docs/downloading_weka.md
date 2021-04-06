
There are two versions of Weka: Weka 3.8 is the latest stable version
and Weka 3.9 is the development version. New releases of these two versions
are normally made once or twice a year. For the bleeding edge, it is
also possible to download nightly snapshots of these two versions. 

The stable version receives only bug fixes and feature upgrades that
do not break compatibility with its earlier releases, while the
development version may receive new features that break compatibility
with its earlier releases.

Weka 3.8 and 3.9 feature a package management system that makes it
easy for the Weka community to add new functionality to Weka. The
package management system requires an internet connection in order to
download and install packages.

# Snapshots
              
Every night, a snapshot of the Subversion repository with the Weka
source code is taken, compiled, and put together in ZIP files. This
happens for both the development branch of the software and the stable
branch.  Those who want the latest bug fixes before the next official
release is made can download these
[snapshots](https://www.cs.waikato.ac.nz/~ml/weka/snapshots/weka_snapshots.html).

# Stable version

Weka 3.8 is the latest stable version of Weka. This branch of Weka
only receives bug fixes and upgrades that do not break compatibility
with earlier 3.8 releases, although major new features may become
available in packages.  There are different options for downloading
and installing it on your system:

### Windows

* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-8-5-azul-zulu-windows.exe)
to download a self-extracting executable for 64-bit Windows that
includes Azul's 64-bit OpenJDK Java VM 11 (weka-3-8-5-azul-zulu-windows.exe; 124.6 MB)

This executable will install Weka in your Program Menu. Launching via the Program
Menu or shortcuts will automatically use the included JVM to run Weka.

### Mac OS - Intel processors
                  
* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-8-5-azul-zulu-osx.dmg)
to download a disk image for Mac OS that contains a
Mac application including Azul's 64-bit OpenJDK Java VM 11 for Intel Macs.
(weka-3-8-5-azul-zulu-osx.dmg; 151.8 MB)

### Mac OS - ARM processors
                  
* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-8-5-azul-zulu-arm-osx.dmg)
to download a disk image for Mac OS that contains a
Mac application including Azul's 64-bit OpenJDK Java VM 11 for ARM Macs.
(weka-3-8-5-azul-zulu-arm-osx.dmg; 144 MB)

### Linux

* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-8-5-azul-zulu-linux.zip)
to download a zip archive for Linux that includes Azul's 64-bit OpenJDK Java VM 11
(weka-3-8-5-azul-zulu-linux.zip; 137.4 MB)

First unzip the the zip file. This will create a new directory called
weka-3-8-5. To run Weka, change into that directory and type

``` bash
./weka.sh
```

### Other platforms

* Click [here](https://prdownloads.sourceforge.net/weka/weka-3-8-5.zip)
 to download a zip archive containing Weka (weka-3-8-5.zip; 55.4 MB)

First unzip the zip file. This will create a new directory called
weka-3-8-5. To run Weka, change into that directory and type

``` bash
java -jar weka.jar
```

Note that Java needs to be installed on your system for this to
work. Also note that using `-jar` will override your current
CLASSPATH variable and only use the `weka.jar`.

# Developer version

This is the main development trunk of Weka and continues from the stable Weka 3.8 code line. It
may receive new features that break backwards compatibility.

### Windows

* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-9-5-azul-zulu-windows.exe)
to download a self-extracting executable for 64-bit Windows that
includes Azul's 64-bit OpenJDK Java VM 11 (weka-3-9-5-azul-zulu-windows.exe; 124.4 MB)

This executable will install Weka in your Program Menu. Launching via the Program
Menu or shortcuts will automatically use the included JVM to run Weka.


### Mac OS - Intel processors
                  
* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-9-5-azul-zulu-osx.dmg)
to download a disk image for Mac OS that contains a
Mac application including Azul's 64-bit OpenJDK Java VM 11 for Intel Macs.
(weka-3-9-5-azul-zulu-osx.dmg; 151.6 MB)

### Mac OS - ARM processors
                  
* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-9-5-azul-zulu-arm-osx.dmg)
to download a disk image for Mac OS that contains a
Mac application including Azul's 64-bit OpenJDK Java VM 11 for ARM Macs.
(weka-3-9-5-azul-zulu-arm-osx.dmg; 145 MB)

### Linux

* Click
[here](https://prdownloads.sourceforge.net/weka/weka-3-9-5-azul-zulu-linux.zip)
to download a zip archive for Linux that includes Azul's 64-bit OpenJDK Java VM 11
(weka-3-9-5-azul-zulu-linux.zip; 137.2 MB)

First unzip the the zip file. This will create a new directory called
weka-3-9-5. To run Weka, change into that directory and type

``` bash
./weka.sh
```

### Other platforms

* Click [here](https://prdownloads.sourceforge.net/weka/weka-3-9-5.zip)
 to download a zip archive containing Weka (weka-3-9-5.zip; 55.2 MB)

First unzip the zip file. This will create a new directory called
weka-3-9-5. To run Weka, change into that directory and type

``` bash
java -jar weka.jar
```

Note that Java needs to be installed on your system for this to
work. Also note, that using `-jar` will override your current
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
migrator](https://www.cs.waikato.ac.nz/~ml/weka/modelMigrator.jar)
tool can migrate some models to 3.8 (a known
exception is RandomForest). Usage is as follows:

``` bash 
java -cp <path to modelMigrator.jar>:<path to weka.jar> weka.core.ModelMigrator -i <path to old serialized weka mode> -o <upgraded model file name>
```
