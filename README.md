# ETL Operations on Country-GDP Data

This Python script performs ETL (Extract, Transform, Load) operations on Country-GDP data sourced from a Wikipedia page. It extracts GDP information for various countries, transforms the data, and then loads it into both a CSV file and a SQLite database.

## Dependencies
- BeautifulSoup (`bs4`)
- Requests (`requests`)
- Pandas (`pandas`)
- NumPy (`numpy`)
- SQLite (`sqlite3`)
- Datetime (`datetime`)

## Code Overview
- **Extract**: The script extracts GDP data from a Wikipedia page using BeautifulSoup and stores it in a Pandas DataFrame.
- **Transform**: It transforms the GDP information from currency format to a float value and converts the GDP from USD (Millions) to USD (Billions), rounding to two decimal places.
- **Load**: The transformed data is then loaded into both a CSV file and a SQLite database.

## Usage
1. Ensure all dependencies are installed.
2. Run the script. It will perform the following steps:
    - Extract data from the Wikipedia page.
    - Transform the extracted data.
    - Load the transformed data into a CSV file and a SQLite database.
    - Run a query on the database to filter countries with GDP greater than or equal to 100 billion USD.

## Functions
1. `extract(url, table_attribs)`: Extracts GDP data from the specified URL and returns it as a Pandas DataFrame.
2. `transform(df)`: Transforms the GDP data in the DataFrame and returns the transformed DataFrame.
3. `load_to_csv(df, csv_path)`: Saves the DataFrame to a CSV file.
4. `load_to_db(df, sql_connection, table_name)`: Saves the DataFrame to a SQLite database table.
5. `run_query(query_statement, sql_connection)`: Runs a specified query on the SQLite database table.
6. `log_progress(message)`: Logs progress messages at different stages of the code execution to a log file.

## Example
```python
# Importing required libraries
from bs4 import BeautifulSoup
import requests
import pandas as pd
import numpy as np
import sqlite3
from datetime import datetime

# Define attributes
url = "https://web.archive.org/web/20230902185326/https://en.wikipedia.org/wiki/List_of_countries_by_GDP_%28nominal%29"
table_attribs = ['Country', 'GDP_USD_millions']
dbname = 'World_Economies.db'
table_name = 'Countries_by_GDP'
csv_path = 'Countries_by_GDP.csv'

# Run ETL process
log_progress('Preliminaries complete. Initiating ETL process')
df = extract(url, table_attribs)
log_progress('Data extraction complete. Initiating Transformation process')
df = transform(df)
log_progress('Data transformation complete. Initiating loading process')
load_to_csv(df, csv_path)
log_progress('Data saved to CSV file')
sql_connection = sqlite3.connect(dbname)
log_progress('SQL Connection initiated.')
load_to_db(df, sql_connection, table_name)
log_progress('Data loaded to Database as table. Running the query')
query_statement = f"SELECT * from {table_name} WHERE GDP_USD_billions >= 100"
run_query(query_statement, sql_connection)
log_progress('Process Complete.')
sql_connection.close()
