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


![](F:\work in jupyter\coel_coding_challenge\1.png)


