Having problems getting Weka to run from a DOS/UNIX command prompt? Getting `java.lang.NoClassDefFoundError` exceptions? Most likely your **CLASSPATH** environment variable is not set correctly - it needs to point to the `weka.jar` file that you downloaded with Weka (or the parent of the Weka directory if you have extracted the jar). Under DOS this can be achieved with:

```text
 set CLASSPATH=c:\weka-3-4\weka.jar;%CLASSPATH%
```
Under UNIX/Linux something like:

```bash
 export CLASSPATH=/home/weka/weka.jar:$CLASSPATH
```
An easy way to get avoid setting the variable this is to specify the CLASSPATH when calling Java. For example, if the jar file is located at c:\weka-3-4\weka.jar you can use:

```bash
 java -cp c:\weka-3-4\weka.jar weka.classifiers... 
```
See also the [CLASSPATH](classpath.md) article.