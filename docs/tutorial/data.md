# Data

## Data types

`ipyvizzu` currently supports two types of data series: dimensions and measures.
Dimensions slice the data cube `ipyvizzu` uses, whereas measures are values
within the cube.

Dimensions are categorical series that can contain strings and numbers, but both
will be treated as strings. Temporal data such as dates or timestamps should
also be added as dimensions. By default, `ipyvizzu` will draw the elements on
the chart in the order they are provided in the data set. Thus we suggest adding
temporal data in a sorted format from oldest to newest.

Measures at the moment can only be numerical.

## Adding data

There are multiple ways you can add data to `ipyvizzu`.

- Using pandas DataFrame
- Specify data by series - column after column if you think of a spreadsheet
- Specify data by records - row after row
- Using data cube form
- Using JSON

??? tip
    You should set the data in the first animate call.

    ```python
    chart.animate(data)
    ```

| Genres | Kinds        | Popularity |
| ------ | ------------ | ---------- |
| Pop    | Hard         | 114        |
| Rock   | Hard         | 96         |
| Jazz   | Hard         | 78         |
| Metal  | Hard         | 52         |
| Pop    | Smooth       | 56         |
| Rock   | Smooth       | 36         |
| Jazz   | Smooth       | 174        |
| Metal  | Smooth       | 121        |
| Pop    | Experimental | 127        |
| Rock   | Experimental | 83         |
| Jazz   | Experimental | 94         |
| Metal  | Experimental | 58         |

### Using `pandas` DataFrame

Use
[`add_data_frame`](../reference/ipyvizzu/animation.md#ipyvizzu.animation.Data.add_data_frame)
method for adding data frame to
[`Data`](../reference/ipyvizzu/animation.md#ipyvizzu.animation.Data).

```python
import pandas as pd
from ipyvizzu import Data


data = {
    "Genres": [
        "Pop",
        "Rock",
        "Jazz",
        "Metal",
        "Pop",
        "Rock",
        "Jazz",
        "Metal",
        "Pop",
        "Rock",
        "Jazz",
        "Metal",
    ],
    "Kinds": [
        "Hard",
        "Hard",
        "Hard",
        "Hard",
        "Smooth",
        "Smooth",
        "Smooth",
        "Smooth",
        "Experimental",
        "Experimental",
        "Experimental",
        "Experimental",
    ],
    "Popularity": [
        114,
        96,
        78,
        52,
        56,
        36,
        174,
        121,
        127,
        83,
        94,
        58,
    ],
}
df = pd.DataFrame(data)

data = Data()
data.add_data_frame(df)
```

!!! info
    `ipyvizzu` makes a difference between two types of data, numeric (measure)
    and not numeric (dimension). A column's `dtype` specifies that the column is
    handled as a measure or as a dimension.

It is also possible to add data frame index to
[`Data`](../reference/ipyvizzu/animation.md#ipyvizzu.animation.Data) with the
[`add_data_frame_index`](../reference/ipyvizzu/animation.md#ipyvizzu.animation.Data.add_data_frame_index)
method.

```python
import pandas as pd
from ipyvizzu import Data


df = pd.DataFrame(
    {"Popularity": [114, 96, 78]}, index=["x", "y", "z"]
)

data = Data()
data.add_data_frame(df)
data.add_data_frame_index(df, "DataFrameIndex")
```

#### Using csv

Download `music_data.csv` [here](../assets/data/music_data.csv).

```python
import pandas as pd
from ipyvizzu import Data


df = pd.read_csv(
    "https://ipyvizzu.vizzuhq.com/latest/assets/data/music_data.csv"
)

data = Data()
data.add_data_frame(df)
```

#### Using Excel spreadsheet

Download `music_data.xlsx` [here](../assets/data/music_data.xlsx).

```python
import pandas as pd
from ipyvizzu import Data


df = pd.read_excel(
    "https://ipyvizzu.vizzuhq.com/latest/assets/data/music_data.xlsx"
)

data = Data()
data.add_data_frame(df)
```

#### Using Google Sheets

```python
import pandas as pd
from ipyvizzu import Data


google_sheet_id = "<Google Sheet id>"
worksheet_name = "<Worksheet name>"

df = pd.read_csv(
    f"https://docs.google.com/spreadsheets/d/{google_sheet_id}/gviz/tq?tqx=out:csv&sheet={worksheet_name}"
)

data = Data()
data.add_data_frame(df)
```

For example if the url is
`https://docs.google.com/spreadsheets/d/abcd1234/edit#gid=0` then
`google_sheet_id` here is `abcd1234`.

#### Using SQLite

```python
import pandas as pd
import sqlite3
from ipyvizzu import Data


# establish a connection to the SQLite database
conn = sqlite3.connect("mydatabase.db")
# read data from a SQLite table into a pandas DataFrame
df = pd.read_sql("SELECT * FROM mytable", conn)
# close the connection
conn.close()

data = Data()
data.add_data_frame(df)
```

!!! note
    You'll need to adjust the SQL query and the database connection parameters
    to match your specific use case.

#### Using MySQL

```python
import pandas as pd
import mysql.connector
from ipyvizzu import Data


# establish a connection to the MySQL database
conn = mysql.connector.connect(
    user="myusername",
    password="mypassword",
    host="myhost",
    database="mydatabase",
)
# read data from a MySQL table into a pandas DataFrame
df = pd.read_sql("SELECT * FROM mytable", con=conn)
# close the connection
conn.close()

data = Data()
data.add_data_frame(df)
```

!!! note
    You'll need to adjust the SQL query and the database connection parameters
    to match your specific use case.

#### Using PostgreSQL

```python
import pandas as pd
import psycopg2
from ipyvizzu import Data


# establish a connection to the PostgreSQL database
conn = psycopg2.connect(
    user="myusername",
    password="mypassword",
    host="myhost",
    port="5432",
    database="mydatabase",
)
# read data from a PostgreSQL table into a pandas DataFrame
df = pd.read_sql("SELECT * FROM mytable", con=conn)
# close the connection
conn.close()

data = Data()
data.add_data_frame(df)
```

!!! note
    You'll need to adjust the SQL query and the database connection parameters
    to match your specific use case.

#### Using Microsoft SQL Server

```python
import pandas as pd
import pyodbc
from ipyvizzu import Data


# establish a connection to the Microsoft SQL Server database
conn = pyodbc.connect(
    "Driver={SQL Server};"
    "Server=myserver;"
    "Database=mydatabase;"
    "UID=myusername;"
    "PWD=mypassword"
)
# read data from a SQL Server table into a pandas DataFrame
df = pd.read_sql("SELECT * FROM mytable", con=conn)
# close the connection
conn.close()

data = Data()
data.add_data_frame(df)
```

!!! note
    You'll need to adjust the SQL query and the database connection parameters
    to match your specific use case.

### Specify data by series

When you specify the data by series or by records, it has to be in first normal
form. Here is an example of that:

```python
from ipyvizzu import Data


data = Data()
data.add_series(
    "Genres",
    [
        "Pop",
        "Rock",
        "Jazz",
        "Metal",
        "Pop",
        "Rock",
        "Jazz",
        "Metal",
        "Pop",
        "Rock",
        "Jazz",
        "Metal",
    ],
    type="dimension",
)
data.add_series(
    "Kinds",
    [
        "Hard",
        "Hard",
        "Hard",
        "Hard",
        "Smooth",
        "Smooth",
        "Smooth",
        "Smooth",
        "Experimental",
        "Experimental",
        "Experimental",
        "Experimental",
    ],
    type="dimension",
)
data.add_series(
    "Popularity",
    [114, 96, 78, 52, 56, 36, 174, 121, 127, 83, 94, 58],
    type="measure",
)
```

### Specify data by records

```python
from ipyvizzu import Data


data = Data()

data.add_series("Genres", type="dimension")
data.add_series("Kinds", type="dimension")
data.add_series("Popularity", type="measure")

record = ["Pop", "Hard", 114]

data.add_record(record)

records = [
    ["Rock", "Hard", 96],
    ["Jazz", "Hard", 78],
    ["Metal", "Hard", 52],
    ["Pop", "Smooth", 56],
    ["Rock", "Smooth", 36],
    ["Jazz", "Smooth", 174],
    ["Metal", "Smooth", 121],
    ["Pop", "Experimental", 127],
    ["Rock", "Experimental", 83],
    ["Jazz", "Experimental", 94],
    ["Metal", "Experimental", 58],
]

data.add_records(records)
```

### Using data cube form

<table>
  <tbody><tr><th colspan="2" rowspan="2"></th><th colspan="4" style="text-align:center">Genres</th></tr>
  <tr>
      <td>Pop</td><td>Rock</td><td>Jazz</td><td>Metal</td>
  </tr>
  <tr><th rowspan="3" style="text-align:center">Kinds</th>
      <td style="text-align:center">Hard</td>
      <td>114</td><td>96</td><td>78</td><td>52</td>
  </tr>
  <tr><td style="text-align:center">Smooth</td>
      <td>56</td><td>36</td><td>74</td><td>121</td>
  </tr>
  <tr>
      <td style="text-align:center">Experimental</td>
      <td>127</td><td>83</td><td>94</td><td>58</td>
  </tr>
  <tr><td colspan="2"></td><th colspan="4" style="text-align:center">Popularity</th></tr>
</tbody></table>

```python
from ipyvizzu import Data


data = Data()

data.add_dimension("Genres", ["Pop", "Rock", "Jazz", "Metal"])
data.add_dimension("Kinds", ["Hard", "Smooth", "Experimental"])

data.add_measure(
    "Popularity",
    [
        [114, 96, 78, 52],
        [56, 36, 174, 121],
        [127, 83, 94, 58],
    ],
)
```

### Using JSON

Download `music_data.json` [here](../assets/data/music_data.json) (in this
example the data stored in the data cube form).

```python
from ipyvizzu import Data


data = Data.from_json("../assets/data/music_data.json")
```
