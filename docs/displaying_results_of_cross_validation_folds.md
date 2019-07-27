The following KnowledgeFlow setup outputs the cross-validation models for each train/test pair:

```text
  ArffLoader
    --dataSet--> CrossValidationFoldMaker
    --trainingSet/testSet--> J48
    --text--> TextViewer
```

# Links
* [Cross-validation_folds_output.kfml](files/Cross-validation_folds_output.kfml) - Example KnowledgeFlow layout file
