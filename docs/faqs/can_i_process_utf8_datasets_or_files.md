Java can process UTF-8 files without any problems, it is just that Java uses a different encoding for displaying them under Windows (= "Cp1252"). If you change the file encoding to "utf-8" everything should be fine. If you are running WEKA directly from the commandline, just add the following parameter to your commandline:

> `-Dfile.encoding=utf-8`

If you are starting WEKA from the Start menu, then edit the `RunWEKA.ini` file:

* If a `fileEncoding` placeholder already exists, then just change the value from "Cp1252" to "utf-8" (without the quotes of course).
* If there isn't a `fileEncoding` yet, just add the `-Dfile.encoding=utf-8` parameter to all the `java`/`javaw` commands).
