---
title: Predicting Berry Yield Using ML
---
My [kaggle](https://www.kaggle.com/joshknight/predicting-berry-yield-using-ml-r?kernelSessionId=75420218) where I explore predicting berry yield using agronomic traits data set.

Overview of the datas findings below.

This data set was obtained from [Kaggle](https://www.kaggle.com/saurabhshahane/wild-blueberry-yield-prediction). This is berry simulation data from a research paper (10.17632/p5hvjzsvn8.1) validated over 30 years of field observations. 

#### Summary statistics of the data

```
                     vars   n    mean      sd  median trimmed     mad     min     max   range  skew kurtosis    se
clonesize               1 777   18.77    7.00   12.50   18.25    0.00   10.00   40.00   30.00  0.57    -0.64  0.25
honeybee                2 777    0.42    0.98    0.25    0.35    0.00    0.00   18.43   18.43 16.70   296.31  0.04
bumbles                 3 777    0.28    0.07    0.25    0.28    0.00    0.00    0.58    0.58  0.15     1.68  0.00
andrena                 4 777    0.47    0.16    0.50    0.46    0.18    0.00    0.75    0.75  0.19    -0.67  0.01
osmia                   5 777    0.56    0.17    0.63    0.58    0.18    0.00    0.75    0.75 -0.92     0.54  0.01
MaxOfUpperTRange        6 777   82.28    9.19   86.00   82.31   12.75   69.70   94.60   24.90 -0.01    -1.35  0.33
MinOfUpperTRange        7 777   49.70    5.60   52.00   49.72    7.71   39.00   57.20   18.20 -0.02    -1.34  0.20
AverageOfUpperTRange    8 777   68.72    7.68   71.90   68.75   10.53   58.20   79.00   20.80 -0.02    -1.35  0.28
MaxOfLowerTRange        9 777   59.31    6.65   62.00   59.34    9.19   50.20   68.20   18.00 -0.02    -1.35  0.24
MinOfLowerTRange       10 777   28.69    3.21   30.00   28.70    4.45   24.30   33.00    8.70 -0.01    -1.35  0.12
AverageOfLowerTRange   11 777   48.61    5.42   50.80   48.63    7.41   41.20   55.90   14.70 -0.01    -1.34  0.19
RainingDays            12 777   18.31   12.12   16.00   18.51   18.13    1.00   34.00   33.00 -0.21    -1.24  0.43
AverageRainingDays     13 777    0.32    0.17    0.26    0.32    0.24    0.06    0.56    0.50  0.07    -1.28  0.01
fruitset               14 777    0.50    0.08    0.51    0.51    0.08    0.19    0.65    0.46 -0.52     0.05  0.00
fruitmass              15 777    0.45    0.04    0.45    0.45    0.04    0.31    0.54    0.22 -0.10    -0.47  0.00
seeds                  16 777   36.12    4.38   36.17   36.13    4.56   22.08   46.59   24.51 -0.06    -0.40  0.16
yield                  17 777 6012.85 1356.96 6107.38 6059.28 1417.36 1637.70 8969.40 7331.70 -0.32    -0.39 48.68
```

#### Correlations of the variables

<img src="/assets/img/Cor_Variables.png">

There is some concerning data here. Variables fruitset, fruitmass, and seeds are very close to being yield. So using these variables to predict yield should be giving the models massive accuracy increases which is concerning, but we will proceed.

We will explore five different machine learning algorithms, random forest, GLM, GLM with alpha/lambda tuning, ridge, and lasso. These models are ones I have found to be really good for agricultural data. The data split will be 80/20 training/testing and we will do cross-validation for each model using 5 folds.

“Typically, given these considerations, one performs k-fold cross-validation using k = 5 or k = 10, as these values have been shown empirically to yield test error rate estimates that suffer neither from excessively high bias nor from very high variance.
 
-Page 184, An Introduction to Statistical Learning“

#### Model performance

##### Observed vs Predictions on Testing Data
<img src="/assets/img/Pred_Plots_All_Var.png">

##### RMSE of the models performance over multiple testings
<img src="/assets/img/RMSE_All_Var.png">

##### Rsquare of the models performance over multiple testings
<img src="/assets/img/Rsquar_All_Var.png">

#### Conclusion
Looking at the performances of the models, lasso had the lowest RMSE and highest Rsquare values compared to the other models. This means that using our lasso model trained on this data would produce the best predictions for yield compared to the other models tested here and on this data. 

#### Observations and suggestions
Looking at the trait variables that explained the most variance for lasso, we can see that fruitset explained the greatest variance for yield. 

<img src="/assets/img/Lasso_Var_All.png">

This was common across the other models as well. If we think about this, fruitset is nearly equal to berry yield. To think about this we can say if something is 90% good, this means it will be better than something that is 20% good. Also, fruitset, fruitmass, and seeds seem like traits that need to be physically taken which I think should be avoided to help save time and money. It would be interesting to see how taking these variables out of the data and just using the environmental data would influence the models… Overall these models performed way to good in my option and could be due to it is simulated data and pretty much yield (fruitset, fruitmass, and seeds) was given to the models. 
