# 06.Make the validator

```
# Create the CrossValidator
cv = tune.CrossValidator(estimator=lr,
               estimatorParamMaps=grid,
               evaluator=evaluator
               )
```

# 07. Fit the model(s)

```
# Call lr.fit()
best_lr = lr.fit(training)

# Print best_lr
print(best_lr)
```

# 08. Evaluating binary classifiers

AUC: Area Under the Curve
ROC: Receiver Operating Curve

the closer the AUC is to one (1), the better the model is!


# 09. Evaluate the model

```
# Use the model to predict the test set
test_results = best_lr.transform(test)

# Evaluate the predictions
print(evaluator.evaluate(test_results))
```

# Output:
`0.7123313100891033` 

