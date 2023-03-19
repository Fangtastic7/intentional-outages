# intentional-outages

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

- No Encodings for these features are necessary since these categorical variables are in the right values we want.

**Prediction**: CAUSE.CATEGORY 

- We will encode 'intentional attack' as a 1 and other categories as 0

**Cross-validation Performance**: 0.530085 (degree of 2)

- The mean RMSE for two hyperparameters is pretty low, which shows that it can generalize decently well.

**Prediction Performance**: 

- Accuracy: 0.6996197718631179

- Precision: 0.5403225806451613

- Recall: 0.7528089887640449

- f1-score: 0.6291079812206571

**Confusion Matrix**: 

<iframe src="baseline_model_confusion_matrix.png" width=600 height=400 frameBorder=0></iframe>

## Final Model

## Fairness Analysis