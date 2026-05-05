# Name: Bennie Kibet
# Project : Syria Tel analysis

## Business Understanding
## Overview:

Syriatel telecommunications company operating in Kenya offers a variety of plans to their customers, ranging from international plans, voicemail plans,etc. The company has experienced growth but has a challenge in retaining customers, as they opt out of their services (churn). Acquiring new customers is typically more expensive than retaining existing ones, so reducing churn is a high-priority business goal.

## Problem Statement:
To determine whether we can predict which customers are likely to leave soon, so the company can intervene early.

## Key Business Questions

* Are there patterns in customer behavior that signal churn?
* Which customers are most at risk of leaving soon?
* What factors (e.g., pricing, service usage, complaints) drive churn?

## Metrics:

From a business perspective:

* Reduction in churn rate
* Increase in retained customers
* ROI of retention campaigns

From a model perspective:

* Accuracy - To know how well our model is performing
* Recall (very important) - Catch as many churners as possible
* Precision - Avoid wasting resources on customers who won’t churn
* AUC-ROC - Overall model performance


## Objectives
1. Reduce customer churn
2. Increase customer lifetime value 
3. Improve retention strategies

# Data Understanding

## Summary of the data

The data set has 21 columns of data(features) and 3333 data entries.

1.  state - The state the customer comes from
2.   account length - The account number of the customer
3.   area code - The assigned code by the state indicating area zones
4.   phone number - Unique identifier of customer
5.   international plan - Shows whether customer has subscribed for the international plan
6.   voice mail plan - Shows whether customer has subscribed for the voice mail plan
7.   number vmail messages - The number of voice mail messages received by a cutomer   
8.   total day minutes - The duration of time spent by customer on calls during the day
9.   total day calls - The number of calls made by cutomer during the day  
10.  total day charge - The charges for making a call during the day
11.  total eve minutes - The duration of time spent by customer on calls during the evening
12.  total eve calls - The number of calls made by cutomer during the evening  
13.  total eve charge - The charges for making a call during the evening
14.  total night minutes - The duration of time spent by customer on calls during the night
15.  total night calls - The number of calls made by cutomer during the night 
16.  total night charge - The charges for making a call during the night
17.  total intl minutes - The duration of time spent by customer on international calls
18.  total intl calls - The number of international calls made by cutomers  
19.  total intl charge - The charges for making an international call
20.  customer service calls - Calls received by the customer care 
21.  churn - Likelyhood of a customer stopping to use the service 

## EDA

* The data had no missing values and null values in the data set.

![alt text](Images/hist.png)

### Discussion
* The distributions show that most usage-related variables (e.g., total day/evening/night minutes and charges) are approximately bell-shaped, suggesting stable and consistent customer behavior across time periods. The close alignment between minutes and corresponding charges indicates strong linear relationships, which is useful for models like Logistic Regression.

* Variables such as number of voicemail messages and customer service calls are highly right-skewed, with many customers having low values and a few having very high counts. This suggests that extreme behaviors (e.g., frequent complaints) may be strong indicators of churn.

* The area code feature appears categorical with limited variation, implying it may have low predictive power unless combined with other regional insights. Similarly, account length is fairly normally distributed, indicating customers are spread across different tenure levels without strong imbalance.

* Call counts (day/evening/night) are moderately normally distributed, reflecting consistent calling patterns, while international calls and minutes are skewed toward lower values, indicating fewer users engage heavily in international usage.

* Overall, the dataset appears well-structured with mostly clean distributions, but skewed variables and potential outliers (especially in service calls and voicemail usage) should be carefully handled, as they could carry strong signals for identifying churn behavior.

## Checking for Outliers

![alt text](Images/box_plots.png)

### Discusion

* The box plot shows that several continuous variables, especially total day, evening, and night minutes and charges, contain noticeable outliers, indicating some customers have unusually high usage.
* The box plot for total evening minutes has a higher average compared to the other minutes indicating more calls occur at night that other times(day, evening) which may be due to the lower night charges average compared to other times(day, evening)
* Customer service calls and number of voicemail messages also exhibit outliers, suggesting a subset of customers with extreme behavior.
* Features like area code and binary plan indicators show little variation and minimal outlier presence.
* The presence of outliers suggests the need for robust modeling or potential treatment (e.g., capping or transformation).
* Overall, variability is highest in usage-related features, which may strongly influence churn predictions.

## Bivariate Analysis

In this section, we will perform bivariate analysis to examine the relationship between the target variable - churn and the other numeric and continuous features in the data. We will use scatter plots to show the direction, strength, and shape of the relationship between two numeric variables. This will help us understand how one variable affects or is affected by another variable and identify any patterns or trends that may exist.

![alt text](Images/bivariate.png)

### Discussion

* Customer churn does not depend on a single factor; most variables show significant overlap between churners and non-churners, indicating the need for a combined model approach.
* The strongest signal comes from customer service calls, where higher call frequency is clearly associated with churn.
* Customers with international plans and those with higher usage (especially day minutes/charges) also show a higher tendency to churn.
* Features like area code and account length appear to have little predictive value.
* Overall, churn is mainly driven by customer dissatisfaction and cost-related factors, which the model can leverage for targeted retention strategies.

## Multivariate Analysis

In this section, we will perform multivariate analysis to examine the relationship between the target variable - churn and multiple features in the data. We will use heatmap to visualize the correlation matrix of the features and see how they are related to each other and to the price.

A heatmap can show us the strength and direction of the correlation between two variables using different colors and shades. This will help us identify the most important features for the prediction and avoid multicollinearity problems.

![alt text](Images/heatmap.png)

### Discusion

* The correlation matrix shows strong multicollinearity between usage variables and their corresponding charges (e.g., total minutes and total charges are almost perfectly correlated), indicating redundancy.
* Most features have weak correlation with churn, suggesting no single variable strongly predicts customer loss.
* The most notable relationships with churn are customer service calls, international plan, and total day minutes and day charges, though correlations are still moderate.
* Binary features like international plan and voicemail plan are highly correlated within themselves due to encoding.
* Overall, the model will need to rely on combined feature effects rather than individual predictors for accurate churn prediction.

# Modeling

## Baseline model: Logistic Regression

![alt text](Images/cm_log.png)

### Discussion
* Precision: The precision values for class 0 and class 1 are 0.85 and 0.0, respectively. Higher precision signifies a lower rate of false positives for that class. With a higher precision in class 0, the model demonstrates better performance in predicting class 0 than class 1.
* Recall: The recall values for class 0 and class 1 are 1.0 and 0.0, respectively. Recall measures the model's ability to correctly identify positive instances. Like precision, recall is higher for class 0, indicating better performance in identifying class 0 instances compared to class 1.
* F1-Score: The F1-scores for class 0 and class 1 are 0.92 and 0.0, respectively. The F1-score is the harmonic mean of precision and recall, balancing both metrics. Once again, class 0 has a higher F1-score than class 1.
* Accuracy: The model's accuracy is 0.85, meaning that 85% of the predictions were correct out of all instances.
* The model correctly predicts most class 0 instances (440 correct vs 130 misclassified), indicating strong performance on class 0.
* For class 1, performance is weaker, with 76 correct predictions and 21 misclassified as class 0.
* There is a noticeable bias toward predicting class 0, as seen from higher correct and incorrect counts in that column.
* False positives (130) are relatively high compared to false negatives (21), suggesting overprediction of class 1 when the true class is 0.
* Overall, the classifier performs better on class 0 than class 1, indicating class imbalance or model bias.

>Therefore, logistic regression achieves 85% prediction accuracy on the test data.
>Based on these metrics, it is evident that the model performs better for class 0 compared to class 1.

## Model 2: Logistic Regression with L1 penalty

![alt text](Images/cm_log_1.png)

### Discussion

* The model still performs well on class 0 (436 correct vs 134 misclassified), but errors remain relatively high.
* Class 1 detection is modest, with 75 correct predictions and 22 false negatives.
* False positives (134) are significantly higher than false negatives (22), indicating a tendency to misclassify class 0 as class 1.
* Compared to the previous matrix, performance is nearly unchanged, with a slight drop in correct class 0 predictions and a minor increase in misclassification.
* Overall, the classifier remains biased toward class 0, with weaker sensitivity to class 1.
* Precision: The precision values for class 0 and class 1 are 0.95 and 0.36, respectively. Higher precision signifies a lower rate of false positives for that class. With a higher precision in class 0, the model demonstrates better performance in predicting class 0 than class 1.
* Recall: The recall values for class 0 and class 1 are 0.76 and 0.77, respectively. Recall measures the model's ability to correctly identify positive instances. Like precision, recall is higher for class 0, indicating better performance in identifying class 0 instances compared to class 1.
* F1-Score: The F1-scores for class 0 and class 1 are 0.85 and 0.49, respectively. The F1-score is the harmonic mean of precision and recall, balancing both metrics. Once again, class 0 has a higher F1-score than class 1.
* Accuracy: The model's accuracy is 0.7661169415292354, meaning that 77% of the predictions were correct out of all instances.

>Therefore, logistic regression with L1 penalty achieves 77% prediction accuracy on the test data.
>Based on these metrics, it is evident that the model performs better for class 0 compared to class 1.

## Model 3: Decision tree classifier

![alt text](Images/Dt.png)

### Model Evaluation

![alt text](Images/cm_Dt.png)

### Discussion
* The model shows strong improvement in class 0 prediction (508 correct vs 62 misclassified), significantly reducing false positives.
* Class 1 performance is stable, with 76 correct predictions and 21 false negatives.
* False positives (62) are now much lower than in previous matrices, indicating better precision for class 1 predictions.
* Overall accuracy has improved due to the large gain in correctly classified class 0 samples.
* The model is now more balanced, though class 1 sensitivity still lags behind class 0.
* Precision: The precision values for class 0 and class 1 are 0.96 and 0.55, respectively. Higher precision signifies a lower rate of false positives for that class. With a higher precision in class 0, the model demonstrates better performance in predicting class 0 than class 1.
* Recall: The recall values for class 0 and class 1 are 0.89 and 0.78, respectively. Recall measures the model's ability to correctly identify positive instances. Like precision, recall is higher for class 0, indicating better performance in identifying class 0 instances compared to class 1.
* F1-Score: The F1-scores for class 0 and class 1 are 0.92 and 0.65, respectively. The F1-score is the harmonic mean of precision and recall, balancing both metrics. Once again, class 0 has a higher F1-score than class 1.
* Accuracy: The model's accuracy is 0.8755622188905547, meaning that 88% of the predictions were correct out of all instances.

>Therefore, Decision tree achieves 88% prediction accuracy on the test data.
>Based on these metrics, it is evident that the model performs better for class 0 compared to class 1.

## Model 4: Decision tree with hyperparameter tuning

![Images/Dt_Tuned.png](Images/Dt_Tuned.png)

### Model Evaluation

![alt text](Images/cm_Dt_tuned.png)

### Discussion
* Precision: The precision values for class 0 and class 1 are 0.97 and 0.76, respectively. Higher precision signifies a lower rate of false positives for that class. With a higher precision in class 0, the model demonstrates better performance in predicting class 0 than class 1.

* Recall: The recall values for class 0 and class 1 are 0.96 and 0.81, respectively. Recall measures the model's ability to correctly identify positive instances. Like precision, recall is higher for class 0, indicating better performance in identifying class 0 instances compared to class 1.

* F1-Score: The F1-scores for class 0 and class 1 are 0.96 and 0.79, respectively. The F1-score is the harmonic mean of precision and recall, balancing both metrics. Once again, class 0 has a higher F1-score than class 1.

* Accuracy: The model's accuracy is 0.9355322338830585, meaning that 94% of the predictions were correct out of all instances.

>Therefore, Decision tree achieves 94% prediction accuracy on the test data.
>The model performs the best compared to the other models with a high accuracy as well as precision scores of both the false and true values of y.

# Evaluation

## Analysis accuracy level
* We have developed four machine learning models to predict the possibility of a customer to churn. Upon testing, we found that logistic regression performs poorly, a testing accuracy of 85%. Despite applying a L1 penalty to mitigate overfitting, the testing accuracy dropped to 77%.

* In contrast, the decision tree classifier and decision tree classifier tuned models demonstrated better accuracy. The decision tree achieved training and testing accuracies of 84% and 88%, respectively, while decision tree classifier tuned model had a training accuracy of 89% and a testing accuracy of 93%.

* Therefore, it is evident that the decision tree tuned model had the best average prediction accuracy model. To achieve the best model, we applied hyperparameter tuning on the decision tree classifier model therefore improving our prediction accuracy.

* The decision tree tuned model was preferable as it outperformed all the other models in terms of precision, recall, and F1 score.

![alt text](Images\ROC_curve.png)

### Discussion
* The ROC curves show that Decision tree clearly outperforms the other models with an AUC of 0.57, indicating strong discrimination between classes.
* Both Decision Tree variants (AUC is 0.55–0.56) perform only slightly better than random guessing, suggesting weak predictive power.
* Both the Logistic Regression variants have an AUC of 0.50, essentially equivalent to random performance, indicating it fails to capture useful patterns.
* Overall, Decision tree model is the most reliable model here, while the others may need better tuning, features, or different modeling approaches.

# Conclusion
> By leveraging the best model, which is the Decision tree model, Syriatel telecommunications can achieve significant benefits:

* Accurate Prediction of churns: The model's high accuracy ensures effective identification of customers likely to churn, enabling preparation on how to appease the customer.
* By integrating this model into their customer analytics pipeline, Syriatel can shift from reactive to preventive retention strategies, targeting users before they leave. 
* The model’s solid recall ensures most potential churners are detected, while its overall accuracy supports confident decision-making. 
* Although some false positives exist, they are acceptable in a retention context where early engagement is preferable to losing customers. 

### Recommendations

* Deploy the model in real-time or periodic scoring systems to continuously monitor churn risk.
* Design targeted retention campaigns (discounts, loyalty rewards, personalized offers) for high-risk customers.
* Improve feature engineering by incorporating usage patterns, complaints, and customer service interactions.
* Regularly retrain and validate the model to maintain performance as customer behavior evolves.
* Combine model outputs with business rules (e.g., high-value customers) to prioritize intervention efforts.