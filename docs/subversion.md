# General
The Weka [Subversion](http://en.wikipedia.org/wiki/Subversion_%28software%29) repository is accessible and browseable via the following URL:

* https://svn.cms.waikato.ac.nz/svn/weka/

A Subversion repository has usually the following layout:
```
 root
  |
  +- trunk
  |
  +- tags
  |
  +- branches
```

Where `trunk` contains the `main trunk` of the development, `tags` snapshots in time of the repository (e.g., when a new version got released) and `branches` development branches that forked off the main trunk at some stage (e.g., legacy versions that still get bugfixed).

# Source code
The latest version of the Weka source code can be obtained with this URL:

https://svn.cms.waikato.ac.nz/svn/weka/trunk/weka

If you want to obtain the source code of the book version, use this URL:

https://svn.cms.waikato.ac.nz/svn/weka/branches/stable-3-8/
 
# Specific version
Whenever a release of Weka is generated, the repository gets [tagged](http://en.wikipedia.org/wiki/Revision_tag):

`dev-X-Y-Z`

the tag for a release of the developer version, e.g., `dev-3-9-2` for Weka 3.9.2
   
https://svn.cms.waikato.ac.nz/svn/weka/tags/dev-3-9-2

`stable-X-Y-Z`

the tag for a release of a stable version. The book version is one of those stable versions, e.g., `stable-3-8-2` for Weka 3.8.2.
   
https://svn.cms.waikato.ac.nz/svn/weka/tags/stable-3-8-2

# JUnit
Weka's JUnit tests are no longer a separate module (as it was the case before the migration to Subversion). They are now located in the `src/test` directory of the Weka source code tree.

# Commandline
Modern Linux distributions already come with Subversion either pre-installed or easily installed via the package manager of the distribution. If that shouldn't be case, or if you are using Windows, you have to download the appropriate client from [http://subversion.tigris.org/ Subversion's homepage].

A checkout of the current developer version of Weka looks like this:

```
svn co https://svn.cms.waikato.ac.nz/svn/weka/trunk/weka
```

You can also obtain the source code for a specific date. The `-r` option of the `svn` command-line client can also take dates (format: YYYYMMDD) instead of only revision numbers. In order to distinguish the dates from revision numbers, you have to enclose the date within curly brackets:

```
svn co -r {20180616} https://svn.cms.waikato.ac.nz/svn/weka/trunk/weka
```

# Links

* [Subversion on WikiPedia](http://en.wikipedia.org/wiki/Subversion_%28software%29)
* [Subversion homepage](https://subversion.apache.org/)
* [JUnit homepage](http://www.junit.org/)

