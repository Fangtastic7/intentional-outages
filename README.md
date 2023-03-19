# Intentional Outages

### March 2023

---

## The Problem

Was it intentional? This is a question asked in many situations. You happen to be walking on the street and out of nowhere, someone bumps into you? Was it accidental? In most cases, these moments are often thought to be accidental. But how about in situations, such as a power outage? Power outages don't happen very frequently, and sometimes we really want to know what caused these major disasters to occur. Afterall, electricity makes up most things in our lives. It lights up our lives by providing us with endless sources of entertainment and the opportunity to answer prediction problems, such as guessing whether or not a major power outage was intentional.

Now, when we try to identify whether a power outage was done intentionally, we shall use classification, more specifically - binary classification. When we look at the categories for causes, there are many methods. Besides intentional attacks, we have a large portion of power outages caused by severe weather and sometimes there can be system or mechanical failures. However, the question we are trying to answer is whether the cause was intentional or not, so we only need 1s and 0s. 

**What are we predicting?**: CAUSE.CATEGORY

- The CAUSE.CATEGORY contains the broad categories of how a major power outage occurred. We want to find out what caused this disaster. 'CAUSE.CATEGORY.DETAIL' might not be ideal because sometimes it can be difficult to precisely indicate what type of severe weather or attack was done, but most importantly, we want to know whether it was just a coincidence or a crime.  The features that we would know in time for this prediction is the general climate of that day, where it happened, and when it started. Features like what exactly happened, such as if it was vandalism, might be found out after finding out whether the damages were accidental or not. 

**Metric**: Accuracy

- We can't really utilize recall because when an intentional attack occurs, we can blatantly see damages that are triggered by a human. In a case of a true negative, classifying the cause behind a power outage as a natural disaster one but actually being a case of vandalism can be qutie rare. People wouldn't necessarily be trying to harm other people's electricity when they need the electricity themselves to survive in such harsh weather. For example, if a tree falls on a power line, it would be rare to say that a human did it under hurricane weather. First of all, why would someone go outside in hurricane winds to just cut down a tree and make it fall on the power lines. Otherwise, there will be cut marks or damages to a tree. In most cases, the winds probably blew the tree down. 

- Precision is not the perfect metric in this case. When we look at false positives for identifying intentional attacks, it is first initially predicting that it is intentional, but finding out it is actually caused by a natural force. This scenario is very unlikely because once we find the culprit or person behind the action, it is almost impossible to overturn that decision.

- Accuracy seems like the right choice because if we see someone cause a power outage or find human damages; we will know. Otherwise, we would most likely categorize the cause of a power outage under severe weather if there is a storm that happened during the time of the outage or by some system failure. 

---

## Baseline Model

**Model**: KNeighbors Classifier - Computes k number of neighbors vote. K depends on the nubmer of nearest neighbors (individual points). For each new instance, we calculate the amount of neighbors it has. The most amount of neighbors indicates a category.

**Features**: CLIMATE.CATEGORY (nominal) & CLIMATE.REGION (nominal)

- For these features, I had to use OneHotEncoder to convert the categorical nominal values into arrays, which distinguish each by its own category. For example, if we had colors -red, green, blue- we could annotate them as [1, 0] for red, [0, 1] for green, and [0, 0] for blue. 

**Prediction**: CAUSE.CATEGORY 

- We will encode 'intentional attack' as a 1 and other categories as 0

**Cross-validation Performance**: 0.530085 (degree of 2)

- The mean RMSE for two hyperparameters is pretty low, which shows that it can generalize decently well.

**Prediction Performance**: 

- Accuracy: 0.70

- Precision: 0.54

- Recall: 0.75

- f1-score: 0.63

**Confusion Matrix**: 

<iframe src="baseline_model_confusion_matrix.html" width=800 height=600 frameBorder=0></iframe>

**Performance Analysis**: The accuracy is around 70%, which is pretty good for the model. Climate does have a lot of impact indicating whether or not a power outage occurred when there is a severe storm or whether an intentional attack. During times of normal weather, severe weather is usually not the cause, which reduces the amount of choices the model has to choose from. In addition, different climate regions have their own climate or weather patterns, so the model is able to pick up and learn about these patterns. Thus, the accuracy is pretty high. 

- On the other hand, precision is not that high. Out of all the times we predicted intentinoal attacks, oly about 54% were right. Recall is fairly strong; we guessed right 75% of the time of all intentional attacks. 
As a result, the f1-score will not be very high because it is a balance between recall and precision, but precision does poorly in this case.

---

## Final Model

Now, when we are building our final model, let's add some new features! 

We will now use the time (YEAR, MONTH) and the total amount of customers who were using the electricity at the time of the power outage.

**Features**

1) YEAR - Indicates the year when the outage event occurred

2) MONTH - Indicates the month when the outage event occurred

3) CLIMATE.REGION - U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)

4) CLIMATE.CATEGORY - This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI)

5) CAUSE.CATEGORY - Categories of all the events causing major power outages 

6) TOTAL.CUSTOMERS - Annual number of total customers served in the U.S. state

**Table**

|   YEAR |   MONTH | CLIMATE.REGION     | CLIMATE.CATEGORY   |   CAUSE.CATEGORY |   TOTAL.CUSTOMERS |
|-------:|--------:|:-------------------|:-------------------|-----------------:|------------------:|
|   2014 |       5 | East North Central | normal             |                1 |           2640737 |
|   2010 |      10 | East North Central | cold               |                0 |           2586905 |
|   2012 |       6 | East North Central | normal             |                0 |           2606813 |
|   2010 |      11 | East North Central | cold               |                0 |           2586905 |
|   2010 |       7 | East North Central | cold               |                0 |           2586905 |

**Explanation**: Every year, weather patterns are different and there can be re-occuring cyceles, such as several heat waves happening during a certain time or blizzards happening during the Winter. By using the year and month, we can more feasibly determine the cause of a major power outage. In addition, if we are determining whether damages were intentional, I thought that the amount of total customers could be a leading factor to why someone would commit such a crime. These new features might have some sort of connection, so this why I will implement them into my final model.

**Preprocessing**

- OneHotEncoder - 'CLIMATE.REGION' - Make them into an array that I can compute

- OneHotEncoder - 'CLIMATE.CATEGORY' - Make them into an array that I can work with

- StandardScalar - 'TOTAL.CUSTOMERS' - Since numbers are large, I standardized them

**Final Model**: Once again, we will not use KNeighborsClassifier again, but now with ideal hyperparameters. To find the best hyperparameters, I had to use GridSearch to find the right amount of neighbors. After training the GridSearch, it showed me that 5 was the ideal number of neighbors.

**Cross-validation**: After 5 folds, the average RMSE was 0.374, which is even lower than the base model. This demonstrates that it will generalizes better than the baseline model.

**Results**: 

- Accuracy: 0.848

- Precision: 0.775

- Recall: 0.775

- f1-score: 0.775

**Confusion Matrix**:

<iframe src="final_model_confusion_matrix.html" width=800 height=600 frameBorder=0></iframe>

**Performance Analysis**: The accuracy improved from the baseline model by about 15%. The time of when major power outages occurred is very important because during certain periods overtime, there has been frequent storms, such as hurricanes or frequent drought. These patterns contribute heavily in identifying that the cause would be severe weather rather than intentional attacks. Aside from time, the total customers is a great factor for determining when an intentional attack occurs. Those that want to purposely negatively affect our lives will most likely go for places that will impact a lot of people. As a result, the model was able to more accurately identify intentional attacks.

- As shown in the confusion matrix, the number of false positives and false negatives have dropped down even more in comparison to the baseline model's confusion matrix. This exhibits that our predictions are more reliable than before. These attribute can also be seen through the improvements in precision, recall, and f1-score.

---

## Fairness Analysis

I always wondered if the accuracy of determining whether an intentional attack causing a major power outage is different for certain periods of time, specifically years. As time goes by, our technology becomes even more advanced and I thought that accuracy might actually be higher for the years 2010 and after. So, I decided to conduct a permutation test to see if the there is a difference in accuracy between the years before 2010 and during and after 2010. 

**Null Hypothesis**: Our model is fair. Its accuracy for determining that an intentional attack caused a major power outages before 2010 or during and after 2010 are roughly the same, and any differences are due to random chance.

**Alternative Hypothesis**: Our model is unfair. The accuracy of determining an intentional attack caused a major power outage before 2010 is greater than the accuracy of determining an intentional attack during and after 2010. 

**Evaluation Metric**: Accuracy

**Test Statistic**: Difference in Accuracy (pre 2010 - post 2010)

**Significance level**: 0.01

<iframe src="permutation_test_diff_accuracy.html" width=600 height=400 frameBorder=0></iframe>

**P-value**: 0.99

**Conclusion**: It seems like the difference in accuracy across the two groups is not significant. Most values fall below the observed difference in accuracy. Thus, our model does achieve accuracy parity, showing that our model is fair and the difference in accuracy is due to random chance.  




