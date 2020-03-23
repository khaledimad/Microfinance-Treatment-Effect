# Microfinance-Treatment-Effect

## Creating our treatment variable
We could transform “degree” to create the treatment variable, d. We could make this a binary
variable, so that if a household has no relationships, then d = 0, and if it has 1 or more
relationships, d = 1. However, we could also make the treatment variable, d, to be a continuous
or an ordinal variable. A continuous or ordinal variable would both give us more granularity than
a binary variable and tell us if having more and more relationships increases the chance of a
household to use microfinance. There are 8600+ households with 55 categories of relationships
(ie, “0 relationships”, “1 relationship”, “2 relationships”, etc. See variable “degree_frequency” in
the R code). A continuous treatment variable would give the most granularity. An ordinal
variable, in which we group the relationships into categories (ie, “0 relationships”, “1-10
relationships, “11-20 relationships”, etc) would give less granularity than a continuous variable
but may be easier for a non-technical manager to understand conceptually. In our model, we used
a continuous variable.

## Use Lasso + CV to predict treatment d from our control variables
We have a total of 8622 records. We have split the data as 80% to be a training set and 20% to be
test set. We ran a lasso + cv model with all the x values (factored except for beds and rooms)
with the network degree(continuous value) as our dependent variable.
We got our lambda value as 0.011(where the mse was minimum) and 1 standard deviation away
lambda as 0.030. For out of the sample, we got an r-squared value of 0.054 and an in sample rsquared
value of 0.06.

![Lasso + CV Model on Training Data](https://github.com/khaledimad/Microfinance-Treatment-Effect/blob/master/Images/Image1.png)

The R-square value is very low for both the models. Hence we infer that the variation of the
degree of connection(d) is not explained greatly by our independent variables. But we could
eliminate the impact of the x’s on our treatment(degree of connection) by including the predicted
d_hat values as an independent variable in the model.

## Use predictions above in an estimator for effect of d on loan

![Lasso + CV Model including d, d_hat and x](https://github.com/khaledimad/Microfinance-Treatment-Effect/blob/master/Images/Image2.png)

We included the predicted d_hat values using the previous model as a dependent in this new
lasso model to determine the effect of the treatment(degree of connection) on our dependent(loan
- yes/no). We got our lambda value as 0.00095(where the mse was minimum) and 1 standard
deviation away lambda as 0.0081. The model is more parsimonious at the one standard deviation
away value of lambda.

## Comparing results above to those from a naive lasso (no CV) for loan on d and x. 
For the naive lasso, we could see that many of the independent variables became zero before log
lambda value is -5. The remaining variables didn’t converge to zero till the log lambda increased
to more than -4. Hence the optimal value for lambda is in between these values - 0.0067 and
0.0183 as beyond log lambda -4, most of the variables converge to zero coefficient. We see there
is a difference in terms of lambda value between the naive lasso and the lasso with d_hat as an
independent variable. We get the effect of connections as 0.1626755 from the two-step lasso
model but the naive lasso value is higher than this.

![Naive Lasso Coefficient Plot](https://github.com/khaledimad/Microfinance-Treatment-Effect/blob/master/Images/Image3.png)

## Bootstraping estimator and describing the uncertainty
Bootstrap is a resampling method. Each time we resample from the existing dataset, we get a
coefficient of the estimator “treatment” denoted as beta-hat. If we resample 1000 times, we have
a sampling distribution of beta-hat. Since our empirical dataset is from an observational study,
the bootstrap estimate beta-hat is very much close to the estimate of beta from the random
population. Thus the uncertainty of bootstrapping is appropriate. The data seems slightly right
sekewed.

![Bootstrap](https://github.com/khaledimad/Microfinance-Treatment-Effect/blob/master/Images/Image4.png)
