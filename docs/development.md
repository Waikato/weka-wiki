We are following the Linux model of releases, where an even second
digit of a release number indicates a "stable" release and an odd
second digit indicates a "development" release (e.g., 3.0.x is a
stable release and 3.1.x is a developmental release). If you are
using a developmental release, there may be new features, but it is
entirely possible that these features will be transient and/or
unstable, and backward compatibility of the API and/or models is not
guaranteed. If you require stability for teaching or deployment in
applications, it is best to use a stable release of Weka.

# Source code repository

Weka's source code for a particular release is included in the
distribution when you download it, in a .jar file (a form of .zip
file) called `weka-src.jar`. However, it is also possible to read source
code directly from the [git source code
repository](git.md) for Weka.

# Code credits

The Weka developers would like to thank [The
MathWorks](http://www.mathworks.com/) and the [National Institute of
Standards and Technology (NIST)](http://www.nist.gov/) for developing
the [Jama Matrix package](http://math.nist.gov/javanumerics/jama/) and
releasing it to the public domain, and to [CERN (European Organization
for Nuclear Research)](http://www.cern.ch/) for statistics-related
code from their Jet libraries (now part of
[COLT](http://dsd.lbl.gov/~hoschek/colt/)).

The core Weka distributions include third-party library code from the
[MTJ project](https://github.com/fommil/matrix-toolkits-java) for fast
matrix algebra in Java, the [Java CUP
project](http://www2.cs.tum.edu/projects/cup/index.php) for generating
parsers, the authentication dialog from the [Bounce
project](http://www.edankert.com/bounce/index.html), and the [Apache
Commons Compress](http://commons.apache.org/proper/commons-compress/)
library. For more information, see the lib folder of the source code
repository.

Weka, including the early non-Java predecessors of Weka 3, was
developed at the [Department of Computer
Science](https://www.cs.waikato.ac.nz/) of the [University of
Waikato](https://www.waikato.ac.nz) in
[Hamilton](https://en.wikipedia.org/wiki/Hamilton,_New_Zealand), [New
Zealand](https://en.wikipedia.org/wiki/New_Zealand). Most of Weka 3
was written by Eibe Frank, Mark Hall, Peter Reutemann, and Len Trigg,
but many others have made significant contributions, in particular,
Remco Bouckaert, Richard Kirkby, Ashraf Kibriya, Xin Xu, and Malcolm
Ware. For complete info on the contributors, check the Javadoc
extracted from the source code of Weka, which is part of the available
[documentation](documentation.md).

Weka's package manager provides access to a large collection of
optional libraries, many of which have been contributed by developers
from other institutions. For information on the authors of these
packages and the third-party libraries used within those Weka
packages, please consult the Javadoc for the relevant package and the
corresponding package lib folder.