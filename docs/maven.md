[Maven](https://maven.apache.org/) is another build tool. But unlike [Ant](ant.md), it is a more high-level tool. Though its configuration file, [pom.xml](https://maven.apache.org/pom.html) is written in XML as well, Maven uses a different approach to the build process. In Ant, you tell it where to find Java classes for compilation, what libraries to compile against, where to put the compiled ones and then how to combine them into a jar. With Maven, you only specify dependent libraries, a compile and a jar plugin and maybe tweak the options a bit. For this to work, Maven enforces a strict [directory structure](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html) (though you can tweak that, if you need to).

# So why another build tool?

Whereas Ant scripts quite often create a *fat jar*, i.e., a jar that contains not only the project's code, but also the contain of libraries the code was compiled against. Handy if you only want to have a single jar. However, this is a nightmare, if you need to update a single library, but all you have is a single, enormous jar. Maven handles [dependencies automatically](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html), relying on libraries (they call them artifacts) to be publicly available, e.g., on [Maven Central](http://search.maven.org/). It allows you to use newer versions of libraries than defined by the dependent libraries (e.g., critical bug fixes), without having to modify any jars manually. Though Maven can also generate *fat jar* files, it is not considered good practice, as it defeats Maven's automatic version resolution.

In order to make Weka, and most of its packages, available to a wider audience (e.g., other software developers), we also publish on Maven Central.

# Compiling
For compiling Weka, you would issue a command like this (in the same directory as `pom.xml`):

```bash
mvn clean install
```

If you don't want the tests to run, use this:
```bash
mvn clean install -DskipTests=true
```

