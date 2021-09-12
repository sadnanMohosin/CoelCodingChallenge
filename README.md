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
![](https://github.com/sadnanMohosin/CoelCodingChallenge/blob/main/images/2.PNG)


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




