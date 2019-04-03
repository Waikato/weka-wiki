Most Java virtual machines only allocate a certain maximum amount of memory to run Java programs. Usually, this is much less than the amount of RAM in your computer. There is some information on default heap sizes in Oracle Java virtual machines for Java 8 [here](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gc-ergonomics.html). However, you can extend the memory available for the virtual machine by setting appropriate options. With Oracle's JDK, for example, you can go

```
java -Xmx2g ...
```

to set the maximum Java heap size to 2GB.

A reliable way to set the maximum heap size for Oracle Java virtual machines (and overwrite any other settings that might be provided in startup scripts, etc.) is to use the _JAVA_OPTIONS environment variable to specify the `-Xmx` option. There is more information [here](http://stackoverflow.com/questions/28327620/difference-between-java-options-java-tool-options-and-java-opts).
