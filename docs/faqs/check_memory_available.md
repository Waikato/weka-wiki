You can easily check, how much memory WEKA can use (this depends on the *maximum heap size* the [[Java Virtual Machine]] was started with).

* SimpelCLI

    * start the SimpleCLI
    * run the following command:

        `java weka.core.SystemInfo`
        
    * the property `memory.max` lists the maximum amount of memory available to WEKA

* GUIChooser


    * select *Help -> SystemInfo*
    * the property `memory.max` lists the maximum amount of memory available to WEKA

In case you should run into an `OutOfMemoryException`, you will have to increase the *maximum heap size*. How much you can allocate, depends heavily on the operating system and the underlying hardware, see the [[Java Virtual Machine]] article). Also, have a look at the [OutOfMemoryException](OutOfMemoryException.md) section further down.
