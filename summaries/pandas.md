# Pandas  

**Category:** Data Handling / Traffic Analysis  

**What it is:**  
Pandas is a Python library for structured data analysis. It allows you to efficiently load, filter, transform, and summarize datasets — commonly used for analyzing logs, network traffic, and other tabular data in cybersecurity workflows.  

**Use cases:**  
- Filtering and summarizing network traffic logs  
- Parsing structured CSV, JSON, or Excel files  
- Identifying anomalous or blocked traffic  
- Preparing datasets for further analysis or visualization

## 📖 Table of Contents

- [Example Snippets](#-example-snippets)
  - [Basic Usage](#basic-usage)
  - [Workflow Example: Network Traffic Analysis](#workflow-example-network-traffic-analysis)
  - [Notes/Tips](#notes/tips)


---

## 🔹 Example Snippets  

### Basic Usage
```python
import pandas as pd  

# Sample Data
data = {  
    'Name': ['Alice', 'Bob', 'Charlie', 'David'],  
    'Age': [25, 30, 35, 40],  
    'City': ['New York', 'London', 'Paris', 'Tokyo']  
}  

df = pd.DataFrame(data)  # Create DataFrame

# Preview data
print(df.head())          # First 5 rows
print(df.info())          # Info about columns & types
print(df.describe())      # Basic stats

# Access columns
names = df['Name']              # Single column
name_age = df[['Name','Age']]   # Multiple columns

# Filtering
older_students = df[df['Age'] > 30]
filtered_df = df[(df['Age'] > 30) & (df['City'] == 'Tokyo')]
filter_or = df[(df['Age'] > 35) | (df['City'] == 'Paris')]
```

### Workflow Example: Network Traffic Analysis
```Python
# Load dataset from external source
url = "https://www.github.com/file.csv" 
df = pd.read_csv(url) 

# Preview data
print(df.head(5))

# Dataset dimensions
print(f"Rows:Columns: {df.shape}")

# Column info
print(df.columns)
print(df.info())
print(df.describe())

# Filter columns of interest
network_activity = df[['source_ip','destination_ip','port','bytesIN','bytesOUT']]
print(network_activity.head(5))

# Conditional filters
blocked_traffic = df[df['status'] == 'BLOCKED']
high_data_transfer = df[df['bytesOUT'] > 5000]

# Summarize results
blocked_summary = blocked_traffic[['timestamp','source_ip','destination_ip','username','category']]

# Remove column
df = df.drop(columns=['column_to_remove'])
print(df.head(5))

# Re-order columns
df = df[[['col1', 'col2', 'col3', 'col4']]
print(df)
```
### Notes/Tips
- Always check for missing or malformed data before filtering
- Combine multiple conditions with & (AND) or | (OR).
- Use .loc[] and .iloc[] for precise row/column selection.
- Ideal for quickly analyzing logs or network capture data before moving to more specialized tools.
- If working with JSON, remember to extract the keys you want to work with before loading into DataFrame
