# CoelCodingChallenge
## Activity Contribution
`To  complete the activity contribution task we need to first import essential libraries like:`
```python
import numpy as np
import pandas as pd 
import matplotlib.pyplot as plt
import seaborn as sns
import datetime
from datetime import date
```

`after reading the activity.csv file we can see four columns in it named date,customer,activity_typw,activity_count`

* Lets find out how many times each activites were done individually with customers

```python
distinct_activity_count = activity.groupby('activity_type').sum()
distinct_activity_count = distinct_activity_count.sort_values(by = 'activity_count',ascending = False)
distinct_activity_count
````

`This code allows us to group activities according to count of activity and sort them to descending order `

![github-small](https://github.com/sadnanMohosin/CoelCodingChallenge/blob/main/images/1.PNG)
>From 9-1-2020 the above number of times each activites is done with customers. "m" activities is the highest which was done 141826 times,then "l" and "x" activities were performed 105307 and 87210 times respectively.

* Now lets find out average time of closing a deal with customers

`Lets read the target.csv file and merge with activity.csv file to find out activity start date and end date`
```python
#merging activity and target csv files
target_activity=pd.merge(target,activity,on = 'customer',how = 'inner')
target_activity.dtypes
```
![2](https://github.com/sadnanMohosin/CoelCodingChallenge/blob/main/images/2.PNG)


`we can see that date_x and date_y is object but we need them as datetime to find the difference between activity starting  and end date.Also change the date_x and date_y column name to activity_start and activity_end respectively`

`Then run the below code to create a new column name "deals_ends_in_days" and difference of days to end deal with customer from the starting date of activity`

```python
#finding out difference between start date and end date
target_activity['deal_ends_in_Days']= target_activity.activity_start - target_activity.activity_end
target_activity['deal_ends_in_Days'] = target_activity['deal_ends_in_Days']/ np.timedelta64(1,'D')
target_activity.head()
```

![3](https://github.com/sadnanMohosin/CoelCodingChallenge/blob/main/images/3.PNG)


`Average the "deals_ends_in_days" column: `

```python
print('Average time to complete a deal is around ',round(target_activity.deal_ends_in_Days.mean(),0),'days')
```

> Average time to complete a deal is around  40.0 days. Which is almost over a month and this performance should be improved

### Visualization
![4](https://github.com/sadnanMohosin/CoelCodingChallenge/blob/main/images/4.PNG)


> From the bar chart we can see that "m" was done 141826 times which is almost 34.68% more than activity "l"

![5](https://github.com/sadnanMohosin/CoelCodingChallenge/blob/main/images/5.PNG)


> But from this graph we can see that though "m" activity was done more than 34% times with the customers to close the deal but "l" activity has the highest success rate to close deals with customers which is almost 2.5%

**To improve performance of the company "l" activity should be done more with customers as we can see it is working fine to close a deal in respect to other activities**

* Finally transfer the activity contribution data to csv file

`Lets merge the activity and target dataframe with left join`

```python
result_all = pd.merge(activity,target,on='customer',how='left',indicator=True)
result_all
```

![6](https://github.com/sadnanMohosin/CoelCodingChallenge/blob/main/images/6.PNG)

> Here in the _merge column "left_only" represents those customers to whom activity has been made but deal not done yet. "both" represents those customers to whome deal has been done.

`now create a column named 'contribution' and assign value 1 for 'both' and 0 for 'left_only'. Then transfer 'activity_type' and 'contribution' column data ta a csv file`

```python
result_all['contribution'] = np.where(result_all['_merge']=="left_only",0,1)
result_all = result_all.loc[:,['activity_type','contribution']]
result_all.to_csv('result.csv',index=False)
```

## Prediction

> Random Forest Algorithm overview

Random forest is a flexible, easy to use machine learning algorithm that produces, even without hyper-parameter tuning, a great result most of the time. It is also one of the most used algorithms, because of its simplicity and diversity (it can be used for both classification and regression tasks).

Random forest is a supervised learning algorithm. The "forest" it builds, is an ensemble of decision trees, usually trained with the “bagging” method. The general idea of the bagging method is that a combination of learning models increases the overall result.

The random forest uses many trees, and it makes a prediction by averaging the predictions of each component tree. It generally has much better predictive accuracy than a single decision tree and it works well with default parameters. If I keep modeling, I can learn more models with even better performance, but many of those are sensitive to getting the right parameters.

> Why I choose the algorithm?

1. One big advantage of random forest is that it can be used for both classification and regression problems, which form the majority of current machine learning systems.
2. Random forest adds additional randomness to the model, while growing the trees. Instead of searching for the most important feature while splitting a node, it searches for the best feature among a random subset of features. This results in a wide diversity that generally results in a better model.
3. Another great quality of the random forest algorithm is that it is very easy to measure the relative importance of each feature on the prediction. 

> Random Forest with given dataset

with random forest this dataset will provide better prediction. As it searches for the best feature among a random subset of features,it will find the best feature to predict deal closing date.

> What informations are missing in dateset

For now there is only one variable available and that is activity_count. Only depending on activity count prediction model will not work well. We need other variables like benefits customers get,cost to complete a deal etc. If we get other variables then random forest will find the best feature to predict closing date.



