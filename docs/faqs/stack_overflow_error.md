Try increasing the stack of your virtual machine. With Sun's JDK you can use this command to increase the stacksize:

```bash
 java -Xss512k ...
```
to set the maximum Java stack size to 512KB. If still not sufficient, slowly increase it.

For Windows, see [OutOfMemoryException](OutOfMemoryException.md) for pointers on how to modify your setup.