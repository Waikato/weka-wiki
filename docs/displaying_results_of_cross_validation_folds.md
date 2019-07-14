The following KnowledgeFlow setup outputs the cross-validation models for each train/test pair:

```text
  ArffLoader
    --dataSet--> CrossValidationFoldMaker
    --trainingSet/testSet--> J48
    --text--> TextViewer
```

# See also
* [Generating cross-validation folds](generating_cross_validation_folds.md)

# Links
* [Cross-validation_folds_output.kfml](files/Cross-validation_folds_output.kfml) - Example KnowledgeFlow layout file
