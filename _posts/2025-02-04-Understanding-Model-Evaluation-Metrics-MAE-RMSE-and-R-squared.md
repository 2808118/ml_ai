---
layout: post
title: 2025.02.13 Understanding Model Evaluation Metrics MAE RMSE and R squared  
tags: R, Machine Learning
categories: R, Machine Learning
---
When building regression models, it's crucial to evaluate their performance using appropriate metrics. Three of the most commonly used metrics are **Mean Absolute Error (MAE)**, **Root Mean Squared Error (RMSE)**, and **R-squared (R²)**. But what do these metrics actually measure? Are they about accuracy, precision, or something else entirely? In this blog, we’ll break down these metrics, explain their formulas, and clarify how they help us assess model performance.  

---

## Blog Structure  
1. **Introduction to Model Evaluation Metrics**  
2. **Mean Absolute Error (MAE)**  
   - What is MAE?  
   - Formula and Interpretation  
3. **Root Mean Squared Error (RMSE)**  
   - What is RMSE?  
   - Formula and Interpretation  
   - How is it different from MAE?  
4. **R-squared (R²)**  
   - What is R-squared?  
   - Formula and Interpretation  
5. **Accuracy vs. Precision: What Do These Metrics Measure?**  
6. **Conclusion: Choosing the Right Metric**  

---

## 1. Introduction to Model Evaluation Metrics  
Regression models are used to predict continuous outcomes, but how do we know if a model is performing well? Metrics like MAE, RMSE, and R-squared help us quantify the model's performance. While MAE and RMSE focus on **accuracy** (how close predictions are to actual values), R-squared measures the **goodness of fit** (how well the model explains the variability in the data). Let’s dive deeper into each metric.  

---

## 2. Mean Absolute Error (MAE)  
### What is MAE?  
MAE measures the average absolute difference between predicted and actual values. It’s a straightforward metric that gives you an idea of how far off your predictions are, on average.  

### Formula  
![image](https://github.com/user-attachments/assets/65dcebb8-e16a-4f83-808b-b41198eabfbc)

### Interpretation  
- MAE is a measure of **accuracy**.  
- It’s less sensitive to outliers compared to RMSE.  
- A lower MAE indicates better model performance.  

---

## 3. Root Mean Squared Error (RMSE)  
### What is RMSE?  
RMSE measures the square root of the average squared differences between predicted and actual values. It penalizes larger errors more heavily than MAE.  

### Formula  
![image](https://github.com/user-attachments/assets/8e4fa028-937b-4467-8e90-547fc5a82e1a)

### Interpretation  
- RMSE is also a measure of **accuracy**, but it’s more sensitive to outliers.  
- A lower RMSE indicates better model performance.  

### How is it different from MAE?  
- RMSE gives more weight to large errors, making it more suitable for cases where outliers are significant.  
- MAE is more robust to outliers and provides a simpler interpretation of average error.  

---

## 4. R-squared (R²)  
### What is R-squared?  
R-squared measures the proportion of the variance in the dependent variable that is predictable from the independent variables. It tells us how well the model explains the variability in the data.  

### Formula  
![image](https://github.com/user-attachments/assets/6af5fd23-b2bf-4438-bffa-29533cbb7ac4)
 
### Interpretation  
- R-squared ranges from 0 to 1.  
  - \(R^2 = 1\): The model explains all the variability in the data (perfect fit).  
  - \(R^2 = 0\): The model explains none of the variability.  
- A higher R-squared indicates better model performance.  

---

## 5. Accuracy vs. Precision: What Do These Metrics Measure?  
- **Accuracy**: Refers to how close the predicted values are to the actual values. MAE and RMSE are measures of accuracy.  
- **Precision**: Refers to the consistency of predictions (how close the predicted values are to each other). R-squared can be loosely related to precision in the sense that it measures how well the model captures the variability in the data.  

---

## 6. Conclusion: Choosing the Right Metric  
- Use **MAE** if you want a simple, interpretable measure of average error that’s robust to outliers.  
- Use **RMSE** if you want to penalize larger errors more heavily.  
- Use **R-squared** to understand how well your model explains the variability in the data.  

By understanding these metrics, you can better evaluate your regression models and choose the one that best fits your needs.  

---
