# Importing Data in Python
## Working with relational databases in Python

### Workflow of SQL querying
- Import packages and functions
- Create the database engine
- Connect to the engine
- Query the database
- Save query results to a DataFrame
- Close the connection


### Creating a database engine
```
from sqlalchemy import create_engine
engine = create_engine('sqlite:///Northwind.sqlite')
```

### Getting table names

```
 table_names = engine.table_names() 
  print(table_names)
  
['Categories', 'Customers', 'EmployeeTerritories',
'Employees', 'Order Details', 'Orders', 'Products',
'Region', 'Shippers', 'Suppliers', 'Territories']
```

### SQL query

```
In [1]: from sqlalchemy import create_engine
In [2]: import pandas as pd
In [3]: engine = create_engine('sqlite:///Northwind.sqlite')
In [4]: con = engine.connect()
In [5]: rs = con.execute("SELECT * FROM Orders")
In [6]: df = pd.DataFrame(rs.fetchall())
In [7]: df.columns = rs.keys()
In [8]: con.close()
```


```
In [1]: from sqlalchemy import create_engine
In [2]: import pandas as pd
In [3]: engine = create_engine('sqlite:///Northwind.sqlite')
In [4]: with engine.connect() as con:
 ...: rs = con.execute("SELECT OrderID, OrderDate,
ShipName FROM Orders")
 ...: df = pd.DataFrame(rs.fetchmany(size=5))
 ...: df.columns = rs.keys()
```


### The pandas way to query
 
```
df = pd.read_sql_query("SELECT * FROM Orders", engine)
```
  
## Interacting with APIs to import data from the web
  
### Loading JSONs in Python
  
```
In [1]: import json
In [2]: with open('snakes.json', 'r') as json_file:
   ...: json_data = json.load(json_file)

In [3]: type(json_data)
Out[3]: dict
```

### Connecting to an API in Python

```
In [1]: import requests
In [2]: url = 'http://www.omdbapi.com/?t=hackers'
In [3]: r = requests.get(url)
In [4]: json_data = r.json()
In [5]: for key, value in json_data.items():
   ...: print(key + ':', value)
```