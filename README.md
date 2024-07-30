# Bank_Marketing

## Problem Statement
A Portuguese banking institution wants to predict the effectiveness of their marketing campaign via phone calls. We are povided with a dataset of 20 features and 1 dependent variable, called `y`. The methods we are using include: K Nearest Neighbors, Random Forest. For processing data, we are also attempting Principle Component Analysis and Feature Selection to reduce the dimension of the dataset.

## Data Analysis

  <a href="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image1.png" target="_blank">
    <img src="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image1.png" alt="image1" width="1000">
</a>

Some initial views about our data:
* Accross **41,188** customers, **88.73%** does not subscribe to the company program and **11.27%** does.
* `pdays`: In 41k of observation, almost 40k observations were not contacted previously.
* `age`: it has quite huge age range, from around 15 to 95. However, most of records fall into the 25-55 range
* `day_of_week`: The distribution of this is quite of a uniform distribution, which may be the indicator that 


## Feature Engineering

### Converting categorical features:
These columns are converted into numerical data to be more feasible for the machine:
* `job`,`marital`,`education`,`housing`,`loan`,`contact`,`month`,`day_of_week`, `poutcome`, `y` using `LabelEncoder.`
* We also group `basic.9y`,`basic.6y`,`basic.4y` in `education` field to `middle_school` to reduce the complexity of the data

We now proceed to investigate the correlation between each column: 

  <a href="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image2.png" target="_blank">
    <img src="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image2.png" alt="image2" width="1000">
</a>

## Modeling
### Process:
After the feature engineering process, we are following this pipeline:
* **Train-test split**: We do 70-30 train-test split
* **Standardize features**: This step is necessary for later use of **PCA**
* **K-Nearest Neighbor (KNN(**: This is an unspervised learning model that takes a lot of time to learn. After running some simulation, `k=37` or at 37 nearest points to the test data yields the most accuracy rate, at `91.15%`. Anything more than this tends to take too long to train, or `prone to underfitting`
  * ROC curve shows `0.93` which is a decent score.
    
   <a href="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image3.png" target="_blank">
    <img src="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image3.png" alt="image3" width="600">
</a>
  *  
  
   <a href="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image4.png" target="_blank">
    <img src="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image4.png" alt="image4" width="600">
</a>

* **Random Forest Classifier (RFC)**: We are able to achieve slightly higher accuracy rate, at `91.65%`
  
   <a href="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image5.png" target="_blank">
    <img src="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image5.png" alt="image5" width="600">
</a>

* **Feature Importance**: After training our model, we can find the importance of each feature in helping the model correctly predict `y.` The top 5 driving factors are:
  * `duration`
  * `euribor3m`
  * `age`
  * `nr.employ`
  * `job`

  <a href="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image6.png" target="_blank">
    <img src="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image6.png" alt="image6" width="600">
</a>
 
### Model optimization:
The following approaches can be conducted to help improve our model:
* **Feature Selection**: For KNN, can deliberately choose features that contribute a lot to our model in the above picture (`>=0.05`)
  * Performance remains the same, with **91.16%** accuracy score, at `K = 35` for **K-NN** model
  * This greatly improve the inference time of the model
* **PCA**: For both KNN and RFC, this method is relevant in reducing the dimension of our fields. With around `14` features, the space of `20` features can be `99%` explained.
  * Performance remains the same, with **91.16%** accuracy score.
  * For such huge dataset, such reduction also helps reduce inference time.
 
    <a href="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image7.png" target="_blank">
    <img src="https://github.com/KietKat/Bank_Marketing/blob/master/Bank_Marketing_image/image7.png" alt="image7" width="800">
</a>

## Conclusion
With 2 fairly simple models, we achieve more than `91%` of accuracy when predicting the successfulness of the campaign. I believe this is a fairly good accuracy regarding how many metrics we have.

* A reality check, duration is a feature that is captured after the call and it is highly influence the outcome of y, which can be expressed as `if duration == 0 then y == 0.` This makes sense, because if a recipient reject the call, i.e refuse to learn more about the campaign, they are very likely not interested in getting involved. Therefore, excluding this feature will yield a more realistic model.
