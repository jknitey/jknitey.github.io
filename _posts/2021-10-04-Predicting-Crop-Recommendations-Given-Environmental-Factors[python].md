---
title: Predicting Crop Recommendations Given Environmental Factors [python]
---
Here we will explore a data set to predict which crop you should plant given specific environmental factors using python and scikit learn. The factors in the data include, nitrogen, phosphorus, potassium, temperature, humidity, pH, and rainfall. There is a total of 22 crops tested and include, lentil, maize, banana, papaya, chickpea,  kidneybeans, jute, cotton, apple, coffee, grapes, mango, rice, mungbean, watermelon, coconut, orange, muskmelon, pigeonpeas, blackgram, mothbeans, and pomegranate. We will look at the data and test a handful of machine learning classification models to see how well we can explain the data.

My [Kaggle](https://www.kaggle.com/joshknight/predicting-crop-recommendations) of the code for this project.

#### Data Structure
We can take a quick look at the data to see how it is structured. We can see the heading includes N, P, K, temperature, humidity ph, rainfall, and the label. Also, we can see the data consists of 2200 rows and 8 columns.

```
	N	P	K	temperature	humidity	ph	rainfall	label
0	90	42	43	20.879744	82.002744	6.502985	202.935536	rice
1	85	58	41	21.770462	80.319644	7.038096	226.655537	rice
2	60	55	44	23.004459	82.320763	7.840207	263.964248	rice
3	74	35	40	26.491096	80.158363	6.980401	242.864034	rice
4	78	42	42	20.130175	81.604873	7.628473	262.717340	rice
...	...	...	...	...	...	...	...	...
2195	107	34	32	26.774637	66.413269	6.780064	177.774507	coffee
2196	99	15	27	27.417112	56.636362	6.086922	127.924610	coffee
2197	118	33	30	24.131797	67.225123	6.362608	173.322839	coffee
2198	117	32	34	26.272418	52.127394	6.758793	127.175293	coffee
2199	104	18	30	23.603016	60.396475	6.779833	140.937041	coffee
2200 rows × 8 columns
```

Next, we want to check for na or zero values which we can see below there are none in the data.
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 2200 entries, 0 to 2199
Data columns (total 8 columns):
 #   Column       Non-Null Count  Dtype  
---  ------       --------------  -----  
 0   N            2200 non-null   int64  
 1   P            2200 non-null   int64  
 2   K            2200 non-null   int64  
 3   temperature  2200 non-null   float64
 4   humidity     2200 non-null   float64
 5   ph           2200 non-null   float64
 6   rainfall     2200 non-null   float64
 7   label        2200 non-null   object 
dtypes: float64(4), int64(3), object(1)
```

Taking a look at each crop, we can see that each crop has 100 entries and which crops are included in the data. This also shows us that the data will be balanced per each crop.
```
lentil         100
maize          100
banana         100
papaya         100
chickpea       100
kidneybeans    100
jute           100
cotton         100
apple          100
coffee         100
grapes         100
mango          100
rice           100
mungbean       100
watermelon     100
coconut        100
orange         100
muskmelon      100
pigeonpeas     100
blackgram      100
mothbeans      100
pomegranate    100
Name: label, dtype: int64
```

To understand the variables we can simply write out how each variable is structured. This also can show us missing data and outliers or bad data. Looking over each variable, there do not seem to be any weird things happening. Ex. no extreme values in the data that look out of place.
```
	N	P	K	temperature	humidity	ph	rainfall
count	2200.000000	2200.000000	2200.000000	2200.000000	2200.000000	2200.000000	2200.000000
mean	50.551818	53.362727	48.149091	25.616244	71.481779	6.469480	103.463655
std	36.917334	32.985883	50.647931	5.063749	22.263812	0.773938	54.958389
min	0.000000	5.000000	5.000000	8.825675	14.258040	3.504752	20.211267
25%	21.000000	28.000000	20.000000	22.769375	60.261953	5.971693	64.551686
50%	37.000000	51.000000	32.000000	25.598693	80.473146	6.425045	94.867624
75%	84.250000	68.000000	49.000000	28.561654	89.948771	6.923643	124.267508
max	140.000000	145.000000	205.000000	43.675493	99.981876	9.935091	298.560117
```
#### Model Exploration

Since we are trying to predict crops for a given environment, we want to look at classification models to explore. These models will suggest which crop should be planted in a given environment unlike a regression model which more or less predict values. We split the data using an 80/20 training/testing split which is fairly standard. We will look at the performance of seven models including, ridge regression (Ridge), Support Vector Classification (SVR), Stochastic Gradient Descent (SGDC), Nearest Neighbors (KNN), Gaussian Processes (GPC), Gaussian Naive Bayes (BaysNa), and Decision Trees (Tree). To evaluate and compare each model performance, we will use the [sklearn.metrics.accuracy_score]( https://scikit-learn.org/stable/modules/model_evaluation.html#accuracy-score) which will give us a metric to find the best model.

Running each model, we can see that the Gaussian Naive Bayes model had a “perfect” classification score followed by SVR, KNN, and Tree. This is quite good but we want to check this performance using some visual modeling.
```
Ridge
Accuracy score: 0.63
SVR
Accuracy score: 0.98
SGDC
Accuracy score: 0.74
KNN
Accuracy score: 0.98
GPC
Accuracy score: 0.64
BaysNa
Accuracy score: 1.00
Tree
Accuracy score: 0.98
```

 Below is a simple plot between the true test sample values and the BaysNa model predicted values for each test sample. We can see there is at least one or more values miss classified shown by the plotting deviation along the diagonal line.
<img src="/assets/img/Class_Crop_Enviro1.png">

We can count the miss classifications to see how well the Gaussian Naive Bayes model performed:
```Number of mislabeled points out of a total 440 points : 1```
Using the testing data there was 1 miss classification which is super good!

We can also plot the confusion matrix as a heat map which indicates the miss classified labels. We can see that the one miss classification was between jute and rice. However, overall amazing predictions but the Gaussian Naive Bayes model for this data.

<img src="/assets/img/Class_Crop_Enviro2.png">

#### Summary
In summary, we explored predicting crop recommendations using environmental factors across seven different machine learning models. We found that using a Gaussian Naive Bayes model best predicted the data and this model could now be used to accurately provide crop recommendations give environmental factors!
