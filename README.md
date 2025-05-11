# How to find the None values in pandas dataframe and get the index and column? 

```python

# we can try this :
import pandas as pd
data = {'name': ['m', 'a', None, 'y'], 'age':[30, 28, 26, None]}
df = pd.DataFrame(data=data)

# this will give df with index, col as index and true or false as values 
null_val = df.isna().stack()

# this will return only the true as (MultiIndex) then we convert it to list so we will have list of tuples at end 
null_val = null_val[null_val].index.tolist()
print(null_val)
# out --> [(2, 'name'), (3, 'age')]
```

# How to run your Project in Rancher Kubernetes Cluster ?


