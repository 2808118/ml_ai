---
layout: post
title: 2025.01.28 Hyperparameter Tuning for Random Forest in R: A step by step guide
tags: R programming
categories: R, Machine Learning
---
## **Table of Contents**
1. [Introduction](#introduction)
2. [Why Hyperparameter Tuning?](#why-hyperparameter-tuning)
3. [Step 1: Define the Hyperparameter Grid](#step-1-define-the-hyperparameter-grid)
4. [Step 2: Set Up Cross-Validation](#step-2-set-up-cross-validation)
5. [Step 3: Train the Model](#step-3-train-the-model)
6. [Step 4: Analyze the Results](#step-4-analyze-the-results)
7. [Step 5: Optimize the Grid](#step-5-optimize-the-grid)
8. [Step 6: Save the Best Model](#step-6-save-the-best-model)
9. [Conclusion](#conclusion)
10. [Full Code Example](#full-code-example)

---

## **Introduction**
Hyperparameter tuning is a critical step in building effective machine learning models. It involves finding the optimal combination of hyperparameters to maximize model performance. In this blog, we’ll walk through how to create a **hyperparameter grid** for a Random Forest model using the `ranger` package in R and use cross-validation to find the best hyperparameter values.

---

## **Why Hyperparameter Tuning?**
Hyperparameters are settings that control the behavior of a machine learning algorithm. Unlike model parameters, which are learned during training, hyperparameters must be set before training begins. Choosing the right hyperparameters can significantly improve model performance, while poor choices can lead to underfitting or overfitting.

For Random Forest models, key hyperparameters include:
- **`mtry`**: Number of variables randomly sampled at each split.
- **`splitrule`**: Splitting rule for decision trees.
- **`min.node.size`**: Minimum size of terminal nodes.
- **`num.trees`**: Number of trees in the forest.
- **`sample.fraction`**: Fraction of observations to sample for each tree.
- **`replace`**: Whether to sample observations with replacement.

---

## **Step 1: Define the Hyperparameter Grid**
The first step is to create a grid of hyperparameter values to test. We’ll use the `expand.grid` function in R to generate all possible combinations of hyperparameters.

```R
# Define the hyperparameter grid
tuning_grid <- expand.grid(
  mtry = c(2, 4, 6),                   # Number of variables to sample
  splitrule = c("variance", "extratrees"), # Splitting rule
  min.node.size = c(1, 5, 10),         # Minimum node size
  num.trees = c(100, 200, 500),        # Number of trees
  sample.fraction = c(0.5, 0.7, 1.0),  # Fraction of observations to sample
  replace = c(TRUE, FALSE)             # Sampling with or without replacement
)
```

This grid will test:
- 3 values of `mtry` (2, 4, 6),
- 2 values of `splitrule` (`"variance"`, `"extratrees"`),
- 3 values of `min.node.size` (1, 5, 10),
- 3 values of `num.trees` (100, 200, 500),
- 3 values of `sample.fraction` (0.5, 0.7, 1.0),
- 2 values of `replace` (`TRUE`, `FALSE`).

This results in **324 combinations** of hyperparameters.

---

## **Step 2: Set Up Cross-Validation**
To evaluate the performance of each hyperparameter combination, we’ll use **k-fold cross-validation**. The `caret` package in R makes this easy with the `trainControl` function.

```R
library(caret)

# Define cross-validation settings
train_control <- trainControl(
  method = "cv",       # Use k-fold cross-validation
  number = 5,          # Number of folds
  verboseIter = TRUE,  # Show progress during training
  savePredictions = "final", # Save predictions for the final model
  search = "grid"      # Use grid search for hyperparameter tuning
)
```

Here, we’re using **5-fold cross-validation**, meaning the data is split into 5 subsets, and the model is trained and validated 5 times, each time using a different subset for validation.

---

## **Step 3: Train the Model**
Next, we’ll train the Random Forest model using the `train` function from the `caret` package. We’ll pass the hyperparameter grid (`tuning_grid`) and cross-validation settings (`train_control`) to the function.

```R
library(ranger)

# Train the Random Forest model
rf_model <- train(
  form = target ~ .,               # Formula for the model
  data = training_data,            # Training data
  method = "ranger",               # Use the ranger package
  trControl = train_control,       # Cross-validation settings
  tuneGrid = tuning_grid,          # Hyperparameter grid
  metric = "MAE",                  # Metric to optimize (e.g., MAE for regression)
  importance = "impurity"          # Measure variable importance
)
```

---

## **Step 4: Analyze the Results**
After training, we can analyze the results to find the best hyperparameter combination.

### **Print the Model Results**
```R
print(rf_model)
```

This will display the performance metrics for each hyperparameter combination and highlight the best-performing model.

### **Plot the Performance**
```R
plot(rf_model)
```

This plot visualizes the performance across different hyperparameter combinations, helping you understand how each hyperparameter affects the model.

### **Extract the Best Hyperparameters**
```R
best_hyperparameters <- rf_model$bestTune
print(best_hyperparameters)
```

This will give you the optimal hyperparameter values for your model.

---

## **Step 5: Optimize the Grid**
If the grid is too large (e.g., 324 combinations), you can reduce it by:
1. **Limiting the range of values**: Test fewer values for each hyperparameter.
2. **Using random search**: Instead of testing all combinations, randomly sample a subset of the grid.
3. **Using Bayesian optimization**: Use advanced methods like `mlrMBO` or `tune` to efficiently search the hyperparameter space.

#### Example: Smaller Grid
```R
tuning_grid <- expand.grid(
  mtry = c(2, 4),                     # Fewer values for mtry
  splitrule = c("variance"),          # Only one splitting rule
  min.node.size = c(1, 5),            # Fewer values for min.node.size
  num.trees = c(100, 200),            # Fewer values for num.trees
  sample.fraction = c(0.7, 1.0),      # Fewer values for sample.fraction
  replace = c(TRUE)                   # Only one value for replace
)
```

---

## **Step 6: Save the Best Model**
Once you’ve identified the best hyperparameters, save the final model for future use.

```R
saveRDS(rf_model, "random_forest_model.Rds")
```

---

## **Conclusion**
Hyperparameter tuning is a powerful technique to improve the performance of your Random Forest models. By systematically testing different hyperparameter combinations and using cross-validation to evaluate their performance, you can find the optimal settings for your specific dataset.

In this blog, we walked through:
1. Creating a hyperparameter grid using `expand.grid`.
2. Setting up cross-validation with `trainControl`.
3. Training the model with `train`.
4. Analyzing the results to find the best hyperparameters.
5. Saving the final model for future use.

By following these steps, you can build more accurate and robust machine learning models. Happy tuning!

---

## **Full Code Example**
```R
library(caret)
library(ranger)

# Define the hyperparameter grid
tuning_grid <- expand.grid(
  mtry = c(2, 4, 6),
  splitrule = c("variance", "extratrees"),
  min.node.size = c(1, 5, 10),
  num.trees = c(100, 200, 500),
  sample.fraction = c(0.5, 0.7, 1.0),
  replace = c(TRUE, FALSE)
)

# Define cross-validation settings
train_control <- trainControl(
  method = "cv",
  number = 5,
  verboseIter = TRUE,
  savePredictions = "final",
  search = "grid"
)

# Train the model
rf_model <- train(
  form = target ~ .,
  data = training_data,
  method = "ranger",
  trControl = train_control,
  tuneGrid = tuning_grid,
  metric = "MAE",
  importance = "impurity"
)

# Print results
print(rf_model)
plot(rf_model)

# Save the model
saveRDS(rf_model, "random_forest_model.Rds")
```
