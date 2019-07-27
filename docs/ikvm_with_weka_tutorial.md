This tutorial walks you through the creation of a *Microsoft C#* program that uses *Weka*, and some *Java* API classes, via *IKVM*. The process will be similar for other *.NET* languages.

# Set up / Installation

You will first need to install IKVM, which can be found [here](http://www.ikvm.net/). You will also need a C# compiler/VM - [Mono](http://www.mono-project.com/) is an excellent open source solution for both linux and windows, or you could just use Microsoft Visual Studio .NET.

# Conversion from Java to a .NET dll

With that out of the way, the first thing you will want to do is to convert the Weka .jar file into a .NET dll. To do this, we will use *ikvmc*, which is the IKVM static compiler.
On the console, go to the directory which contains weka.jar, and type:

```bash
> ikvmc -target:library weka.jar
```
The `-target:library` call causes *ikvmc* to create a .dll library instead of an executable.

Note that the IKVM tutorial tells you that you should add `-reference:/usr/lib/IKVM.GNU.Classpath.dll`(or appropriate path) to the above command, it tells IKVM where to find the GNU Classpath library. **However, `IKVM.GNU.Classpath.dll` Is no longer included in the download package, and is from very old versions of IKVM. When Sun open sources Java, it got replaced by the IKVM.OpenJDK.*.dll files.**

You should now have a file called "weka.dll", which is a .NET version of the entire weka API. That's exactly what we want!

# Use the dll in a .NET application
To try it out, lets use a small C# program that I wrote. The program simply runs the J48 classifier on the Iris dataset with a 66% test/data split, and prints out the correctness percentage. It also uses a few Java classes, and is already about 95% legal Java code.

The code is here:

```csharp
//start of file Main.cs
 using System;

 class MainClass
 {
     public static void Main(string[] args)
     {
         Console.WriteLine("Hello Java, from C#!");
         classifyTest();
      }

      const int percentSplit = 66;
      public static void classifyTest()
      {
         try
         {
             weka.core.Instances insts = new weka.core.Instances(new java.io.FileReader("iris.arff"));
             insts.setClassIndex(insts.numAttributes() - 1);

             weka.classifiers.Classifier cl = new weka.classifiers.trees.J48();
             Console.WriteLine("Performing " + percentSplit + "% split evaluation.");

             //randomize the order of the instances in the dataset.
                         weka.filters.Filter myRandom = new weka.filters.unsupervised.instance.Randomize();
             myRandom.setInputFormat(insts);
                         insts = weka.filters.Filter.useFilter(insts, myRandom);

             int trainSize = insts.numInstances() * percentSplit / 100;
             int testSize = insts.numInstances() - trainSize;
             weka.core.Instances train = new weka.core.Instances(insts, 0, trainSize);

             cl.buildClassifier(train);
             int numCorrect = 0;
             for (int i = trainSize; i < insts.numInstances(); i++)
             {
                 weka.core.Instance currentInst = insts.instance(i);
                 double predictedClass = cl.classifyInstance(currentInst);
                 if (predictedClass == insts.instance(i).classValue())
                     numCorrect++;
             }
             Console.WriteLine(numCorrect + " out of " + testSize + " correct (" +
                        (double)((double)numCorrect / (double)testSize * 100.0) + "%)");
         }
         catch (java.lang.Exception ex)
         {
             ex.printStackTrace();
         }
     }

 }
 //end of file Main.cs
```

# Compile and run it
Now we just need to compile it. If you are using MonoDevelop or Visual Studio, you will need to add references to weka.dll, and all of the IKVM.OpenJDK.*.dll files, and lastly IKVM.Runtime.dll into your project. Otherwise, on the command line, you can type:

*NOTE: 
replace *IKVM.OpenJDK.*.dll with the remaining IKVM.openJDK files.*
```bash
>mcs Main.cs -r:weka.dll,IKVM.Runtime.dll,IKVM.OpenJDK.core.dll, IKVM.OpenJDK.*.dll
```

to run the Mono C# compiler with references to the appropriate dlls (according to the Mono documentation, the command line arguments for Visual Studio are the same).

And there you go! Now you can run the program. But make sure that the Iris.arff dataset is in the same directory first.

For mono:

```bash
>mono Main.exe
```
or if you are using visual studio, just:

```bash
>Main.exe
```
Hopefully you will get as output:

```
 Hello Java, from C#!
 Performing 66% split evaluation.
 49 out of 51 correct (96.078431372549%)
```
And there you have it. Now we have a working program that uses Weka classes, and some classes from the standard Java API, in a C# program for the .NET framework.

# Links
* [An Introduction to IKVM](http://www.onjava.com/pub/a/onjava/2004/08/18/ikvm.html)
* [IKVM.NET](http://www.ikvm.net/)
* [Mono](http://www.mono-project.com/)
* [The official IKVM tutorial](http://www.ikvm.net/userguide/tutorial.html)
* [Use Weka with the Microsoft .NET Framework](use_weka_with_the_microsoft_net_framework.md)