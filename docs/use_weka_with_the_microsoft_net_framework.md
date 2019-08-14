So you want to use Weka with your existing Microsoft .NET code?  Or you want to use your .NET components in a Java-based system that uses Weka?  Achieving .NET and Java interoperability is possible, but there is no 'one size fits all' solution.  The sheer number of different ways to attempt this should give an indication of the difficulty of the problem.

That said, here is a summary of some of the possibilities that you can try.

# Direct Interoperability

## IKVM
[IKVM](http://www.ikvm.net/) is an implementation of Java for .NET.  It allows you to call your Java classes from directly from your .NET code, and via the [GNU Classpath](http://www.gnu.org/software/classpath/classpath.html) provides most of the standard Java API for use in .NET.  It also provides a .NET version of the Java Virtual Machine.

In other words, with this software you can now use almost any Java class in your .NET code!  You could even develop for .NET using Java, and then easily import your classes into your .NET system.

Even better, IKVM is Open Source software and is freely available.  The only disadvantage is that its functionality is limited to the extent of the GNU Classpath; however this now covers most of the API, and is rapidly expanding.  It also doesn't appear to have any functionality in the other direction - you can't run .NET code in Java.

For use with Weka, IKVM has successfully been tested on a simple C# program that runs a classifier on a dataset.  The GUI will not load at the time of writing, but I suspect that most of the Weka API will work fine.  Because of this, IKVM is recommended for use in small Open Source research projects using Weka.  See this [IKVM with Weka tutorial](ikvm_with_weka_tutorial.md) for more detail.

## Bridging Software
Bridging software allows you to use your Java classes in your .NET code, and your .NET classes in your java code.  This works by running both the .NET and Java Virtual Machines simultaneously, and creating proxy classes that 'stand in' for each class in the alternative framework.

Runtime bridges are relatively computationally-efficient, and provide seamless and flexible interoperability solutions.  The main disadvantage with this method is that the software tools that facilitate this are generally expensive third-party programs that must be purchased.

Some .NET / Java bridging tools:

* [JNBridge](http://www.jnbridge.com)
* [JBind2.net](http://www.j2dotnet.com/)
* [JuggerNET](http://www.codemesh.com/en/juggernetproduct_page.html)
* [J-Integra for .NET](http://j-integra.intrinsyc.com/products/net/)
* [JACOB](http://sourceforge.net/projects/jacob-project/)
> An open-source project that provides allows you to call COM components from Java (but not vice-versa).
* [Hosting .NET Controls in Java](http://www.devx.com/interop/article/19845)
> This tutorial explains how to write your own custom COM bridges using JNI.  This is similar to what JACOB does, except that you will have to code it yourself.

Java Native Interface SDK for .NET and tools that use it:

* [Object-Oriented JNI for .NET ](http://jni4net.com/)
> This .NET library implements regular JNI SDK in .NET.
* [OOJNI .NET Add-in for MS Visual Studio](https://oojni-add-in-net-csharp-for-vs2005.soft112.com)
> Generates wrappers for java classes from Java Bytecode selected in C++, Managed C++, C#, J#, VB.

# Indirect Interoperability

## Interoperability using a Database 

If both your .NET components and your Java components only need to asynchronously interface with a database, interoperability is very simple.  The components from the different frameworks do not have to know about each other - they just interact with the database as they normally would.  For more detail, see
[Database Interoperability](http://www.devx.com/interop/article/19952/0/page/2).

## Using Enterprise Messaging Services

Messaging services are used to facilitate asynchronous communication between different components in a system.  They provide an API for sending messages between components, and provide security, data integrity checking and error handling to ensure reliable information transfer.  To use these systems for interoperability between .NET and Java, you may need a messaging system for each framework that has the capability of talking to messaging systems from the other framework.

The disadvantage of this type of system is that it may be expensive and difficult to set up.  However, if you already have such a system in place, it makes sense to use it for interoperability purposes.

**.NET Enterprise Messaging Services**

* [Microsoft BizTalk Server](http://www.microsoft.com/biztalk/default.mspx)

**Java Enterprise Messaging Services**

* [IBM Websphere MQ](http://www-306.ibm.com/software/integration/wmq/features/)
* [Fiorano MQ](http://www.fiorano.com/products/fmq/overview.htm)

## Web Services
Web Services are a common method used for achieving interoperability.  The main attractions are that web-based systems are both platform and language independant, and there exist standard protocols to facilitate the communication.

A Web Services based approach generally involves setting up a web server, and some proxy classes in each framework to communicate with this server.  Communication is generally achieved through XML-based protocols such as SOAP.  The problem with this method is that serialization to XML can create large files that must be sent to the web server, so there may be efficiency issues.

[Microsoft WSE](http://msdn.microsoft.com/webservices/webservices/building/wse/) and [JWSDP](http://java.sun.com/webservices/jwsdp/index.jsp) are freely available extensions to .NET and Java respectively, for the purposes of developing web-server based solutions.

Tutorials for using this method for interoperability can be found [here](http://msdn.microsoft.com/webservices/webservices/building/interop/default.aspx?pull=/library/en-us/dnwse/html/wsejavainterop.asp), [here](http://www.devhood.com/tutorials/tutorial_details.aspx?tutorial_id=178) and [here](http://msdn.microsoft.com/webservices/webservices/building/interop/default.aspx?pull=/library/en-us/dnbda/html/interopsun.asp).

# Other Useful Links

* [An introduction to IKVM](http://www.onjava.com/pub/a/onjava/2004/08/18/ikvm.html) - This article supplies a code example of Java/.NET interoperability using IKVM.
* [IKVM.NET](http://www.ikvm.net/) - An open source implementation of Java for .NET (Highly recommended).
* [IKVM with Weka tutorial](ikvm_with_weka_tutorial.md) - A tutorial on using Weka with C# using IKVM.  This tutorial is part of the WekaWiki.
* [Java/.NET Interop: Bridging Muddled Waters](http://www.devx.com/interop/door/18896) - A very comprehensive set of articles on the interoperability problem.  Watch out for the various authors' biases towards their own companies and products though.
* [Microsoft .NET and Java/J2EE Interoperability](http://msdn.microsoft.com/vstudio/java/interop/default.aspx) - An MSDN directory of articles on .NET / Java interopererability.
* [Mono](http://www.mono-project.com/) - An open source .NET development environment, cross platform.
* [Java/.NET Integration as Simple as Possible](http://www.codeproject.com/dotnet/javadotnetintegrate.asp) - the article, which describes the simplest way to embed .NET controls into a Java GUI with OOJNI®
* [Problems using weka from VB .NET in VS 2005](http://zen-turkey.com/blog/default.aspx?id=11&t=slight-problems-using-weka-in-vb-net-in) - you may experience some hiccups when attempting to use an IKVM generated assembly in your VB .Net code in Visual Studio 2005...
