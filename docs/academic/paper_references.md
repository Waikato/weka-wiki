Due to the introduction of the `weka.core.TechnicalInformationHandler` interface it is now easy to extract all the paper references via `weka.core.ClassDiscovery` and `weka.core.TechnicalInformation`.

The [get_wekatechinfo.sh](../files/get_wekatechinfo.sh).

Typical use (after an `ant exejar`) for [BibTeX](http://en.wikipedia.org/wiki/bibtex):

```bash
 get_wekatechinfo.sh -d ../ -w ../dist/weka.jar -b > ../tech.txt
```
(command is issued from the same directory the Weka `build.xml` is located in)

# Links
* [get_wekatechinfo.sh](../files/get_wekatechinfo.sh)
* [Writing a Classifier](../writing_classifier.md)
