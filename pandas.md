## Traffic Analysis with Pandas

##### Simple Filters
```python
import pandas as pd  
  
# Sampla Data  
  
data = {  
    'Name': ['Alice', 'Bob', 'Charlie', 'David'],  
    'Age': [25, 30, 35, 40],  
    'City': ['New York', 'London', 'Paris', 'Tokyo']  
}  
  
df = pd.DataFrame(data) # Create input data into a Dataframe  

print(df.head()) # Prints first 5 rows of df as sample
print(df.info()) # Prints information about the df
print(df.describe()) # Prints description of df (mean, min, max, ect)

names = df['Name'] # Individual column  
  
name_age = df[['Name', 'Age', 'City']] # Parse #2 Columns  
  
index = df.loc[1] # Locating per index  
  
older_students = df[df['Age'] > 30] # Filtering  
  
filtered_df = df[(df['Age'] > 30) & (df['City'] == 'Tokyo')] # AND
filtering_df = df[(df['Age'] > 35) | (df['City'] == 'Paris')] # OR
```

##### Network Traffic Analysis (workflow)

###### Load Dataset from external source
```Python
url = "https://www.github.com/file.csv" 
df = pd.read_csv(url) 
```
###### Preview Data
```Python
print(df.head(5)) 
```
###### Print number of rows & columns
```Python
print(f"Rows:Columns: {df.shape}")
```
###### Print column names
```Python
print("\nColumn Names")
print(df.columns)
```
###### Print basic information of DataFrame
```Python
print("\nDF Info:")
print(df.info())
```
###### Print data description
```Python
print("\nDF Description:")
print(df.describe())
```
###### Filter Columns of Interest
```Python
filter_name = df[['column1', 'column2', 'column3', 'column4']]
filter_name.head(5)
```
###### Filter use-case columns (Example: Network Activity)
```Python
network_activity = df[['source_ip', 'destination_ip', 'port', 'bytesIN', 'bytesOUT']]
print(network_activity.head(5))
```
###### Conditional Filter (Example: Blocked Traffic)
```Python
blocked_traffic = df[df['status'] == 'BLOCKED']
print(blocked_traffic.head(5))
```
###### Conditional Filter (Example: High data transfer)
```Python
high_data_transfer = df[df['bytesOUT] > 5000]
print(high_data_transfer.head(5))
```
###### Summarise the conditional filter results (blocked_traffic)
```Python
blocked_summary = blocked_traffic[['timestamp', 'source_ip', 'destination_ip', 'username', 'category']]
```
###### Remove column from DataFrame
```Python
df = df.drop(columns = ['column_to_remove'])
print(df.head(5))
