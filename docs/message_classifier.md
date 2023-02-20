

In the following you'll find some information about the MessageClassifier from the **2nd** edition of the [Data Mining](http://www.cs.waikato.ac.nz/~ml/weka/book.html) book by Witten and Frank.

# Source code
Depending on the version of the book, download the corresponding version (this article is based on the **2nd** edition):

* 1st Edition: 
[MessageClassifier](https://www.cs.waikato.ac.nz/~ml/weka/example_code/MessageClassifier.java)
* 2nd Edition: 
[MessageClassifier](https://www.cs.waikato.ac.nz/~ml/weka/example_code/2ed/MessageClassifier.java) ([book](https://git.cms.waikato.ac.nz/weka/weka/-/tree/book2ndEd-branch/wekaexamples/src/main/java/wekaexamples/book/MessageClassifier.java), [stable-3.8](https://git.cms.waikato.ac.nz/weka/weka/-/tree/stable-3-8/wekaexamples/src/main/java/wekaexamples/book/MessageClassifier.java), [developer](https://git.cms.waikato.ac.nz/weka/weka/-/tree/main/trunk/wekaexamples/src/main/java/wekaexamples/book/MessageClassifier.java))

# Compiling
* compile the source code like this, if the `weka.jar` is already in your [CLASSPATH](classpath.md) environment variable:

```bash
 javac MessageClassifier.java
```
* otherwise, use this command line (of course, replace `/path/to/` with the correct path on your system):

```java
 javac -classpath /path/to/weka.jar MessageClassifier.java
```
**Note:** The classpath handling is omitted from here on.

# Training
If you run the `MessageClassifier` for the first time, you need to provide labeled examples to build a classifier from, i.e., messages ("`-m`") and the corresponding classes ("`-c`"). Since the data and the model are kept for future use, one has to specify a filename, where the `MessageClassifier` is serialized to ("`-t`").

Here's an example, that labels the message *email1.txt* as *miss*:

```bash
 java MessageClassifier -m email1.txt -c miss -t messageclassifier.model
```
Repeat this for all the messages you want to have classified.

# Classifying
Classifying an unseen message is quite straight-forward, one just omits the class option ("`-c`"). The following call
```bash
 java MessageClassifier -m email1023.txt -t messageclassifier.model
```
will produce something like this:

```text
 Message classified as : 
miss
```
