Here you'll find instructions of how to create an article that describes the classifier you developed, including the upload of the source code itself. As an example, the Weka classifier **[ZeroR](zero_r.md)** is used.

# Preparation
The content of the wiki is available as [repository on GitHub](https://github.com/Waikato/weka-wiki), which also contains a guide on how to build and test the repository using [MkDocs](https://www.mkdocs.org/). Add or modify items as necessary and then do a [pull request](https://help.github.com/articles/about-pull-requests/). See [this link](https://www.mkdocs.org/user-guide/writing-your-docs/) for details on writing articles.

# Article Layout
To provide fast access to a certain classifier, the following topics should be covered in an article about a classifier:

* insert a **full description** of your classifier, whether it handles numerical and/or nominal classes/attributes; in a nutshell, information a potential user needs to decide whether this specific classifier is suitable for a certain problem
```text
 # Description
 Class for building and using a 0-R classifier. Predicts the mean
 (for a numeric class) or the mode (for a nominal class).
```
* add the reference paper, if any
```text
 # Reference
 -none-
```
* add the package the classifier belongs to, that people can adapt their [GenericObjectEditor](generic_object_editor.md) props file and use the classifier within [Weka](http://www.cs.waikato.ac.nz/~ml/weka/).
```text
 # Package
 weka.classifiers.rules
```
* add the link to download the source code, either an internal one, if you uploaded the source code, or an external one, if you want to point to a different resource; here it is an internal one, pointing to **ZeroR.java**
```text
 # Download
 Source code: [ZeroR.java](files/ZeroR.java)
```
* *(optional)* add additional information, e.g., some benchmarks; which is nothing in our case 
```text
 # Additional Information
 -none-
```
* finally, save and deploy the article
