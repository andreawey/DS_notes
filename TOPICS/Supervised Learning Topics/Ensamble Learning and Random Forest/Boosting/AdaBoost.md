A [[Boosting]] technique is AdaBoostin

- In the sequence of trained models, each model tries to correct the predecessor errors
- One way for a new predictor to correct its predecessor is to pay a bit more attention to the training instances that the predecessor underfitted
- This results in new predictors focusing more and more on the hard cases: techniques used by Ada-Boost

<h2>Building an AdaBoost classifier</h2>
- First a base classifier is trained and used to make predictions on the training set 
  -> the relative weight of misclassified training instances is then increased
- A second classifier is trained using the updated weights and again it makes predictions on the training set
  -> Weights are updated and so on

<h2>Prediction</h2>
- Once all predictors are trained, the ensemble makes predictions very much like bagging or pasting, except that predictors have different weights depending on their overall accuracy on the weighted training set
- This sequential learning technique cannot be parallelized since each predictor can only be trained after the previous predictor has been trained and evaluated
  -> it does not scale as well as bagging or pasting


> [!NOTE] Remark
> if your AdaBoost ensemble model is overfitting the training set, you can try
> - reducing the number of estimators
> - more strongly regularizing the base estimator
