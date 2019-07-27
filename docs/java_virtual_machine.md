
The [Java virtual machine](http://en.wikipedia.org/wiki/java_virtual_machine) (JVM) is the platform dependent [interpreter](http://en.wikipedia.org/wiki/interpreter_%28computing%29) of the [Java](http://en.wikipedia.org/wiki/java_%28programming_language%29) [bytecode](http://en.wikipedia.org/wiki/bytecode) (i.e., the [classes](http://en.wikipedia.org/wiki/class_%28computer_science%29)). It translates the bytecode into machine specific instructions.

# Amount of available memory
If you start the virtual machine without any parameters it takes default values for stack and heap. In case you run into [OutOfMemory](faqs/OutOfMemoryException.md) exceptions, try to start your JVM with a bigger maximum heap size. (However, there's a limit, depending on your OS. See the [32-Bit](java_virtual_machine.md#32-bit) and [64-Bit](java_virtual_machine.md#64-bit) sections.)

# 32-bit
With a 32-Bit machine you can address at most 4GB of [virtual memory](http://en.wikipedia.org/wiki/virtual_memory). Different operating systems divide up the memory further into //system/kernel* and *user space*.

From experience, you can achieve the following maximum sizes for the *heap* on Windows and Linux:

* Windows: 
1.4GB
* Linux: 
1.7GB

# 64-bit
Larger heap sizes are available when using 64-bit Java in a conjunction with a 64-bit operating system.

There is more information available [here](http://www.oracle.com/technetwork/java/hotspotfaq-138619.html#gc_heap_32bit).