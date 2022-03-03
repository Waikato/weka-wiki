
You can either perform [cost-sensitive classification](https://waikato.github.io/weka-wiki/search.html?q=cost-sensitive) or try a resampling filter to get a more balanced class distribution:

* [Resample](https://weka.sourceforge.io/doc.dev/weka/filters/supervised/instance/Resample.html) filter (supervised version, to take class distribution into account)
* [SMOTE](https://weka.sourceforge.io/doc.packages/SMOTE/weka/filters/supervised/instance/SMOTE.html) (available through SMOTE package)
