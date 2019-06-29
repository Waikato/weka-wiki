Yes and no. If you're starting from scratch, you might want to consider [Jython](http://www.jython.org/), a rewrite of [Python](http://www.python.org/) to seamlessly integrate with Java. The drawback is, that you can only use the libraries that Jython implements, not others like [NumPy](http://numpy.scipy.org/) or [SciPy](http://www.scipy.org/). The article [Using WEKA from Jython](../using_weka_from_jython.md) explains how to use WEKA classes from Jython and how to implement a new classifier in Jython, with an example of ZeroR implemented in Jython.

An approach making use of the `javax.script` package (new in Java 6) is [Jepp](http://jepp.sourceforge.net/), *Java embedded Python*. Jepp seems to have the same limitations as Jython, not being able to import Scipy or Numpy, but one can import pure Python libraries. The arcticle [Using WEKA via Jepp](../using_weka_via_jepp.md) contains more information and examples.

Another solution, to access Java from within Python applications is [JPype](http://jpype.sourceforge.net/), but It's still not fully matured.

Finally, you can use the **python-weka-wrapper** Python 2.7 library to access most of the non-GUI functionality of Weka (3.9.x):

* [pypi](https://pypi.python.org/pypi/python-weka-wrapper) 
* [github](https://github.com/fracpete/python-weka-wrapper) 

For Python3, use the **python-weka-wrapper3** Python library:

* [pypi](https://pypi.python.org/pypi/python-weka-wrapper3) 
* [github](https://github.com/fracpete/python-weka-wrapper3) 
