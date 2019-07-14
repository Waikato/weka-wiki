

# Format
Format of the cost matrices:

* regular 
```text
 % Rows	Columns
 2	2
 % Matrix elements
 0.0	5.0
 1.0	0.0
```
* [Matlab](http://www.mathworks.com/) single-line format (see also the [Matlab Primer](http://math.ucsd.edu/~driver/21d-s99/matlab-primer1.html#s2))
```text
 [0.0 5.0; 1.0 0.0]
```

# Testing the format
The following code loads a cost matrix and prints its content to the console. Useful, if one wants to test whether the format is correct:

```java
 import weka.classifiers.CostMatrix;
 
 import java.io.BufferedReader;
 import java.io.FileReader;
 
 /**
  * Loads the cost matrix "args[0]" and prints its content to the console.
  *
  * @author FracPete (fracpete at waikato dot ac dot nz)
  */
 public class CostMatrixLoader {
   public static void main(String[] args) throws Exception {
     CostMatrix matrix = new **CostMatrix**(
                           new BufferedReader(new FileReader(args[0])));
     System.out.println(matrix);
   }
 }
```

# See also
* [CostSensitiveClassifier](cost_sensitive_classifier.md)
* [MetaCost](metacost.md)

# Downloads
* [CostMatrixLoader.java](files/CostMatrixLoader.java)
