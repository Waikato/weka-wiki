The most likely reason for this is that your computer does not have direct access to the Internet and Java needs to be told to use a proxy server to access the web. The best way to achieve this is to configure an environment variable that provides the proxy details, e.g.,

```
_JAVA_OPTIONS
```
    
which is read by Oracle Java virtual machines. There is more information on this variable [here](http://stackoverflow.com/questions/28327620/difference-between-java-options-java-tool-options-and-java-opts). Information on how to set environment variables in Windows is [here](http://www.computerhope.com/issues/ch000549.htm). For Mac users, there is a nice program to set environment variables available [here](https://github.com/hschmidt/EnvPane).

Set the value of this variable to

```
-Dhttp.proxyHost=some.proxy.somewhere.net -Dhttp.proxyPort=port
```

where *some.proxy.somewhere.net* needs to be replaced by the name of your proxy server and *port* needs to be replaced by the appropriate port number on the proxy server. Your IT department should be able to give you these details.

This should allow the package manager to connect to the website that hosts the package meta-information. However, if the package manager still cannot connect to the Internet, you can also force it to run in offline mode, by setting the above environment variable to

```
-Dweka.packageManager.offline=true
```

Then, you can download package .zip files manually via your web browser, by navigating to

[http://weka.sourceforge.net/packageMetaData/](http://weka.sourceforge.net/packageMetaData/)

clicking on the link for the package you want to install, then clicking on *Latest*, and finally clicking on the URL given next to *PackageURL*.

Once you have downloaded the package .zip file, open the WEKA package manager, and click on the *File/URL* button in the top-right corner of the package manager window (in the *Unofficial* panel). Then navigate to your package .zip file and install it.

If you are running Weka in *offline* mode, and the packages you are installing have some dependencies on one another, then there can still be some problems due to Weka not being able to verify the dependencies by checking against the central repository. This is usually a problem in the case where Weka has never been able to connect to the internet and thus has not downloaded and established a cache of the central package metadata repository. Fortunately there is a simple work-around to this, as long as you can access the internet via a web browser:

1. Using your web browser, download [http://weka.sourceforge.net/packageMetaData/repo.zip](http://weka.sourceforge.net/packageMetaData/repo.zip)
2. If it doesn't already exist, create the directory `~/wekafiles/repCache`
3. Copy the downloaded `repo.zip` into `~/wekafiles/repCache` and unzip it there
4. Start Weka (use the `weka.packageManager.offline=true` property to speed up the startup process; see [http://weka.wikispaces.com/How+do+I+use+the+package+manager%3F#Package%20manager%20property%20file] for info)
