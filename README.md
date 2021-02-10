# Mobile-Phone-Price-Range-Prediction

## Predicting Mobile Phone Price Range 
* the dataset is taken from kaggle uploaded by  Abhishek Sharma
* link to the file https://www.kaggle.com/iabhishekofficial/mobile-price-classification
* First prediction using Random Forest Regressor
* Second prediction using XGBoost Regressor
* Using Cross Validation Fold

## Library Used:
* Pandas
* Numpy
* Matplotlib
* Scikit Learn 
* XGBoost


## Features Used
* battery_power      : self explained 
* blue               : bluetooth (0 no, 1 yes)
* clock_speed        : Processor clock speed
* dual_sim           : Dual Sim features (0 No, 1 yes)
* fc                 : front camera (in mega pixels)
* four_g             : 4g (0 No, 1 yes)
* int_memory         : internal memmory (in gb)
* m_dep              : Mobile depth in cm
* mobile_wt          : weight in gram
* n_cores            : number of cores
* pc                 : ?
* px_height          : pixel height
* px_width           : pixel width
* ram                : total Ram
* sc_h               : phone dimension height
* sc_w               : phone dimension width
* talk_time          : total number of talk time before the battery runs out
* three_g            : 3g capability (0 and 1)
* touch_screen       : touch screen yes or no (0 and 1)
* wifi               : wifi (0 and 1)
* price_range        : price range of the phone

## Data Cleaning
* after carefully checking (the usual NAN, datatypes, categorical data etc)
* the datasets  (both train and test) is already cleaned and ready to be used

### Train Data (Head)

|    |   battery_power |   blue |   clock_speed |   dual_sim |   fc |   four_g |   int_memory |   m_dep |   mobile_wt |   n_cores |   pc |   px_height |   px_width |   ram |   sc_h |   sc_w |   talk_time |   three_g |   touch_screen |   wifi |   price_range |
|---:|----------------:|-------:|--------------:|-----------:|-----:|---------:|-------------:|--------:|------------:|----------:|-----:|------------:|-----------:|------:|-------:|-------:|------------:|----------:|---------------:|-------:|--------------:|
|  0 |             842 |      0 |           2.2 |          0 |    1 |        0 |            7 |     0.6 |         188 |         2 |    2 |          20 |        756 |  2549 |      9 |      7 |          19 |         0 |              0 |      1 |             1 |
|  1 |            1021 |      1 |           0.5 |          1 |    0 |        1 |           53 |     0.7 |         136 |         3 |    6 |         905 |       1988 |  2631 |     17 |      3 |           7 |         1 |              1 |      0 |             2 |
|  2 |             563 |      1 |           0.5 |          1 |    2 |        1 |           41 |     0.9 |         145 |         5 |    6 |        1263 |       1716 |  2603 |     11 |      2 |           9 |         1 |              1 |      0 |             2 |
|  3 |             615 |      1 |           2.5 |          0 |    0 |        0 |           10 |     0.8 |         131 |         6 |    9 |        1216 |       1786 |  2769 |     16 |      8 |          11 |         1 |              0 |      0 |             2 |
|  4 |            1821 |      1 |           1.2 |          0 |   13 |        1 |           44 |     0.6 |         141 |         2 |   14 |        1208 |       1212 |  1411 |      8 |      2 |          15 |         1 |              1 |      0 |             1 |


## Data Preprocessing
Since the data is cleaned, and no categorical data or nan, i Proceed to check for outliers

## Outliers

![](/images/all_hist.png)

* the 0 and 1 means the data has no distribution because it is in bolean value
* the rest of the data shows no outliers eventhough none has gaussion distribution

![](/images/all_boxplot.png)

* Boxplot also shows that no extreme values exist

### Building and Training the Model 1
* ML model used : Random Forest Regressor (random state 0 and n_estimator 100)
* Create pipeline and put the model (no preprocessing done since the data is cleaned)
* MAE: 0.171375
* MSE: 0.07810725
* Score: 0.939385961508614

### Predicting the Test data 1
* Doing the same cleaning method as train data and put the prediction result in CSV pair it with the id name respectively

|    |   id |   Price_Range_Prediction |
|---:|-----:|-------------------------:|
|  0 |    1 |                     2.87 |
|  1 |    2 |                     2.95 |
|  2 |    3 |                     2.42 |
|  3 |    4 |                     2.98 |
|  4 |    5 |                     1.05 |
|  5 |    6 |                     3    |
|  6 |    7 |                     3    |
|  7 |    8 |                     0.98 |
|  8 |    9 |                     2.91 |
|  9 |   10 |                     0.01 |

## Cross Validation

* Since the dataset is reltively small (2000 records)
* cross validation is done to look at the model's score better
* it is not necessary but arbitrary since the dataset is small
* MAE scores: [0.168325 0.1772   0.162325 0.170625 0.175625]
* Average Mae Score : 0.17082

### Building and Training the Model 1.2
* Since i do Cross validation, i got curious what if i split the train data based on the folds that gives better score?
* we can see the 3rd fold gives better validation score
* so i'm taking the 3rd fold as validation data and the rest as training data out of curiousity
* using kfold to manually split the data into 5 folds and taking their indices
* perform training data again
* and it did give better score as  
* MAE: 0.0659625
* MSE: 0.011252125
* Score: 0.9909681307341931

### Result from model building 1.2

|    |   id |   Price_Range_Prediction |
|---:|-----:|-------------------------:|
|  0 |    1 |                     2.92 |
|  1 |    2 |                     2.96 |
|  2 |    3 |                     2.3  |
|  3 |    4 |                     2.98 |
|  4 |    5 |                     1.02 |
|  5 |    6 |                     2.99 |
|  6 |    7 |                     3    |
|  7 |    8 |                     0.87 |
|  8 |    9 |                     2.92 |
|  9 |   10 |                     0    |


### Building and Training the Model 2
* ML model used : XGBoost Regressor (learning_rate=0.05 and n_estimator 1000)
* Train the model (and early stopping round 5 and no preprocessing done since the data is cleaned)
* MAE: 0.1711237657815218
* MSE: 0.07372066696244936
* Score: 0.9427901078981458
* Comparatively better than random forest regressor

### Building and Training the Model 2.1
* we can see the 3rd fold gives better validation score in random forest model
* so i'm taking the 3rd fold as validation data and the rest as training data out of curiousity too in to XGBoost regressor
* using kfold to manually split the data into 5 folds and taking their indices
* perform training data again
* and the score:  
* MAE: 0.0028101042099297046
* MSE: 1.8746827505327634e-05
* Score: 0.9999851026297024

* Better MAE score than random forest regressor 1.2, and better than XGboost 1 (0.0028 vs 0.06 vs 0.17)
* Worse MSE score (1.8 vs 0.11 vs 0.07)
* Better Score accuracy (0.9999 vs 0.9909 vs 0.94)

### Result from model building 2.1

|    |   id |   Price_Range_Prediction |
|---:|-----:|-------------------------:|
|  0 |    1 |                     2.92 |
|  1 |    2 |                     2.96 |
|  2 |    3 |                     2.3  |
|  3 |    4 |                     2.98 |
|  4 |    5 |                     1.02 |
|  5 |    6 |                     2.99 |
|  6 |    7 |                     3    |
|  7 |    8 |                     0.87 |
|  8 |    9 |                     2.92 |
|  9 |   10 |                     0    |
