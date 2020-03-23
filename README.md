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

![Lasso + CV Model on Training Data](https://github.com/khaledimad/Microfinance-Treatment-Effect/Images/Image1.png)



