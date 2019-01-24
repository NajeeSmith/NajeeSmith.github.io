---
title: "Grid Search and X-Validation"
date: 2019-01-20
excerpt: ""
header:
  overlay_image: images/Classifiers/G/model.png
  overlay_filter: 0.5
  actions:
    - label: "Full Code"
      url: "https://github.com/najeesmith"
---
# Introducution

I was a little bothered by the manual parameter tuning done in my random forest classification, so I decided to look into automated techniques. So returning to my [old project](RF.md), I decided to find better methods of improving the model. An effective method I found was grid search. K-Fold Cross Validation was also used, but only to see how wild the variation can be with training.

# Code
The code from my old project was kept the exact same. The only difference is the addition of the following code blocks:
```python
# Applying k-Fold Cross Validation
from sklearn.model_selection import cross_val_score
accuracies = cross_val_score(estimator = classifier, X = X_train, y = y_train, cv = 10)
mean = accuracies.mean()
std = accuracies.std()
```
Here, cross fold validation was ran 10 iterations of the model, and gathered both the mean and standard deviation as shown below.

<figure class="half">
<a href="/images\Classifiers\G\mean.PNG"><img src="/images\Classifiers\G\mean.PNG"></a>
<a href="/images\Classifiers\G\std.PNG"><img src="/images\Classifiers\G\std.PNG"></a>
    <figcaption>Relatively high mean and low deviation</figcaption>
</figure>

The outcome of the results were good, however I wanted to see if it could be better. This is where grid search came into play.

```python
# Applying Grid Search to find the best model and the best parameters
from sklearn.model_selection import GridSearchCV
parameters = [{'n_estimators': [1, 5, 10, 50], 'criterion': ['entropy', 'gini'], 'max_depth':[1,5,10,50], 'bootstrap':['True','False'], 'warm_start':['True','False'] }]
grid_search = GridSearchCV(estimator = classifier,
                           param_grid = parameters,
                           scoring = 'accuracy',
                           cv = 10,
                           n_jobs = -1)
grid_search = grid_search.fit(X_train, y_train)
best_accuracy = grid_search.best_score_
best_parameters = grid_search.best_params_
```

Other than testing things I normally would change, such as number of trees, I decided to explore the other arguments that I normally leave out. I used the following reasoning for each argument:
* n_estimators - required to allow the classifier to function
* criterion - tests the loss quality
* max_depth - changes how much information is captured through each split
* bootstrap - retries combinations in hopes that some are significant
* warm_start - utilizes previous solutions and adjusts the trees accordingly

# Results

<figure class="half">
<a href="/images\Classifiers\G\Capture.PNG"><img src="/images\Classifiers\G\Capture.PNG"></a>
<a href="/images\Classifiers\G\accuracy.PNG"><img src="/images\Classifiers\G\accuracy.PNG"></a>
    <figcaption>Optimal tuning and increased accuracy</figcaption>
</figure>

Grid search is an marvelous optimization tool. According to the results, the parameters that I normally would've left out actually played a role in finding a more accurate result. Although great, the accuracy started to get really close to overfitting numbers, so the information gathered were taken with a grain of salt.
