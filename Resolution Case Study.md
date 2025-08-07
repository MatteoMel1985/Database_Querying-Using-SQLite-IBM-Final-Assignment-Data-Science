# Datasets employed  

The datasets utilised for the resolution of `FinalProject.ipynb` can be analysed and downloaded from the `CSV` folder of this project, and comes from the following sources:  

## 1. Socioeconomic Indicators in Chicago  

This dataset contains a selection of six socioeconomic indicators of public health significance and a “hardship index,” for each Chicago community area, for the years 2008 – 2012.  

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at:  

https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t   

## 2. Chicago Public Schools  

This dataset shows all school level performance data used to create CPS School Report Cards for the 2011-2012 school year. This dataset is provided by the city of Chicago's Data Portal.  

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at:  

https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t  

## 3. Chicago Crime Data  

This dataset reflects reported incidents of crime (with the exception of murders where data exists for each victim) that occurred in the City of Chicago from 2001 to present, minus the most recent seven days.  

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at:  

https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2  

# Execution  

As we are executing the task in a Jupyer Notebook environment, we must first run the following code to install the required libraries.  

```Python
!pip install pandas
!pip install ipython-sql prettytable 

import prettytable

prettytable.DEFAULT = 'DEFAULT'
```

## 1. `!pip install pandas`  

* The `!` at the start is a Jupyter Notebook-specific feature. It allows to run shell/command-lines directly from a notebook cell.  

* `pip install pandas` is a command that tells pip, the Python package installer, to install the pandas library.  

### Pandas is a powerful data manipulation and analysis library, which is used for working with data tables (DataFrames), handling CSV files, time series, etc.  

## 2. `!pip install ipython-sql prettytable`  

This line installs two libraries at once: `ipython-sql` and `prettytable`.  

* `ipython-sql` is a Jupyter-specific extension allowing to run SQL queries directly within a notebook using magic commands like `%sql`. It is useful when working with databases inside notebooks.  

* `prettytable` is a Python library designed to create nicely formatted tables in plain text. It helps print tables in a readable format, especially when working in the terminal or notebooks.

## 3. `import prettytable`  

*  It is a regular Python import statement that brings the prettytable library into the current Python environment, so as to use its functions/classes.

*  After installing `prettytable`, it must be imported in the code to be actually used.

## 4. `prettytable.DEFAULT = 'DEFAULT'`  

* This line of code ensures that any PrettyTable object created after this line will automatically adopt the default prettytable styling, effectively resetting any previously applied global style changes. This is useful for maintaining consistency or reverting to a known baseline appearance for the tables within a Jupyter Notebook session.  

# Store the datasets in database tables  

To analyze the data using SQL, it first needs to be loaded into SQLite DB. We will create three tables in as under:  

[CENSUS_DATA](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCensusData.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01)  

[CHICAGO_PUBLIC_SCHOOLS](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoPublicSchools.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01)  

[CHICAGO_CRIME_DATA](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCrimeData.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01)  

The following line is used to load the pandas and sqlite3 libraries and establish a connection to `FinalDB.db`.  

```Python
import pandas as pd
import sqlite3

# Connect to (or create) SQLite database

conn = sqlite3.connect("FinalDB.db")
```

## 1. `import pandas as pd`  

* This line imports the `pandas` library and gives it a short alias `pd`.

## 2. `import sqlite3`  

* Imports the built-in Python module `sqlite3`, which facilitates the interaction with SQLite databases.

* SQLite is a lightweight, file-based database system. It doesn’t require a server, as the whole database is stored in a single file. It is great for demos, small projects, and teaching database concepts without needing a full SQL server.

## 3. `conn = sqlite3.connect("FinalDB.db")`  

* This line creates a connection to an SQLite database.

* If the file `FinalDB.db` already exists, it connects to it, and if it doesn’t exist, it creates a new one in the current working directory.

* `conn` is a connection object used to:

    * Run SQL queries;
    * Read/write tables;
    * Commit changes;
    * Close the database when done.  
 
Now we are proceeding by loading the SQL magic module and use the `Pandas` to load the data available in the links above to dataframes. These dataframes will be loaded on the database `FinalDB.db` as required tables.  

Following is the code employed to achieve this end.  

```Python
# URLs of datasets

census_url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCensusData.csv"
schools_url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoPublicSchools.csv"
crime_url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCrimeData.csv"

# Load into DataFrames

census_df = pd.read_csv(census_url)
schools_df = pd.read_csv(schools_url)
crime_df = pd.read_csv(crime_url)

# Load DataFrames into SQLite tables

census_df.to_sql("CENSUS_DATA", conn, if_exists="replace", index=False)
schools_df.to_sql("CHICAGO_PUBLIC_SCHOOLS", conn, if_exists="replace", index=False)
crime_df.to_sql("CHICAGO_CRIME_DATA", conn, if_exists="replace", index=False)
```

## Part 1: Define Dataset URLs  

* The URLs, or `Uniform Resource Locator` of the 3 `csv` tables, which are `census_url`, `schools_url`, and `crime_url` are established, and indicated after the equal sign between quotation marks in the upper section of the code.  

## Part 2: Load Data from URLs into Pandas DataFrames  

* The formula `pd.read_csv(...)` reads a CSV file directly from the URL and loads it into a DataFrame. It gets applied to the 3 tables independently in the following fashion:

    * `census_df = pd.read_csv(census_url)`;
    * `schools_df = pd.read_csv(schools_url)`;
    * `crime_df = pd.read_csv(crime_url)`.
 
## Part 3: Store the DataFrames in SQLite Tables  

As the same formula is applied to the 3 tables by changing varying their names their names, I will only quote the `census_df`.

* `census_df` is the pandas DataFrame that contains the `CENSUS_DATA`, on which is called the `.to_sql()` method.

* `.to_sql(...)` is a pandas method used to write a DataFrame to a SQL database. It works with various database backends: SQLite, PostgreSQL, MySQL, etc.

* `"CENSUS_DATA"` is the name of the SQL table to create in the database.

* `conn` is the connection object to the database, which was created earlier using `conn = sqlite3.connect("FinalDB.db")`.

* `if_exists="replace"` tells pandas what to do if the table `"CENSUS_DATA"` already exists in the database. Other valid option instead of `"replace"` can be `"fail"`, which raises a `ValueError` and `"append"`, which adds the DataFrame’s data to the existing table.

* `index=False`: By default, pandas includes the DataFrame’s index (row labels) as a column in the SQL table. Setting `index=False` means not to include the index when writing to the table (ex: If the DataFrame has an indexes like `0, 1, 2,...`, this option prevents pandas from saving it as an extra column in the table.

If executed correctly, you will see the number `533` appearing at the bottom of the cell. That refers to the number of rows of `crime_df`, the last dataframe that was loaded.  

![533](https://github.com/MatteoMel1985/Relational-Dataset-Images/blob/main/Database%20Querying%20images/533.jpg?raw=true)  

Finally, we can establish a connection between SQL magic module and the database `FinalDB.db` with the following code.  

```Python
# Load SQL magic

%load_ext sql

# Connect SQL magic to the SQLite database

%sql sqlite:///FinalDB.db
```

## Load SQL magic  

* `%load_ext` is used to load notebook extensions which, in this specific case, is loading the SQL extension.

## Connect SQL magic to the SQLite database  

It tells SQL Magic to use SQLite, and connect to the file `FinalDB.db` in the current folder.  

* `%sql`: Activates SQL magic for this line.  

* `sqlite:///FinalDB.db` is a SQLAlchemy-style connection string, which means

    * `sqlite://`: Use the SQLite database engine.
    * `/FinalDB.db`: The relative path to the database file (in this case, in the current directory).
    * The three slashes `///` indicates a relative path (which is a shortcut that tells the system to look for a file relative to the current working directory (usually wherever the notebook file is).
 
Finally, we are now ready to answer the problem's questions and fill out the cells with the proper SQL strings.  

As all the codes were compiled with SQL Magic strings, I will reported them exactly as written in the Jupyter Notebook, with the initial activation line `%sql`. 

# Problem 1  

## Find the total number of crimes recorded in the CRIME table.  

Its resolution is fairly easy, as it is sufficient to employ the `COUNT(*)` function from SQL, which computes the number of rows from the table `CHICAGO_CRIME_DATA`.  

```SQL Magic
%sql SELECT COUNT(*) AS TOTAL_CRIMES FROM CHICAGO_CRIME_DATA;
```

# Problem 2  

## List community area names and numbers with per capita income less than 11000  

To solve this problem, we must focus on the `CENSUS_DATA` table. We'll have to select the attributes `COMMUNITY_AREA_NAME`, `COMMUNITY_AREA_NUMBER`, and `PER_CAPITA_INCOME`. Then, we'll have to set the `WHERE` clause the attribute `PER_CAPITA_INCOME` is < 11000. Finally, we must order our results descendingly by `PER_CAPITA_INCOME`.  

Following is the SQL Magic string employed.  

```SQL Magic
%sql SELECT COMMUNITY_AREA_NAME, COMMUNITY_AREA_NUMBER, PER_CAPITA_INCOME \
FROM CENSUS_DATA WHERE PER_CAPITA_INCOME < 11000 \
ORDER BY PER_CAPITA_INCOME DESC;
```

# Problem 3  

## List all case numbers for crimes involving minors (children are not considered minors for the purposes of crime analysis).  

To answer this query, we must refer to the `CHICAGO_CRIME_DATA` table. Furthermore, we have to search for the word 'minor' among the attributes `PRIMARY_TYPE` and `DESCRIPTION`. To do so, we employ the SQL clause `WHERE`, the operator `LIKE`, and the search pattern `%MINOR%`, which will look for this specific word among the cells of the `PRIMARY_TYPE` and `DESCRIPTION` attributes, as shown below.  

```SQL Magic
%sql SELECT CASE_NUMBER, PRIMARY_TYPE, DESCRIPTION \
FROM CHICAGO_CRIME_DATA \
WHERE ("PRIMARY_TYPE" LIKE '%MINOR%' OR "DESCRIPTION" LIKE '%MINOR%');
```

# Problem 4  

## List all kidnapping crimes involving a child  

To solve this, we employ the same logic as above; however, we will use the search pattern `'%KIDNAPPING%'` for the attribute `PRIMARY_TYPE`, and `'%CHILD%'` for `DESCRIPTION`.  

```SQL Magic
%sql SELECT CASE_NUMBER, PRIMARY_TYPE, DESCRIPTION \
FROM CHICAGO_CRIME_DATA \
WHERE ("PRIMARY_TYPE" LIKE '%KIDNAPPING%' AND "DESCRIPTION" LIKE '%CHILD%');
```

# Problem 5  

## List the kind of crimes that were recorded at schools (no repetitions).  

A slight variation on the previous logic, by introducing the modifier `DISTINCT`, which returns exclusively unique rows (no duplicates). Furthermore, we will have to set the operator `LIKE` and the search patter '`%SCHOOL%'` within the attribute `LOCATION_DESCRIPTION`.

```SQL Magic
%sql SELECT DISTINCT CASE_NUMBER, PRIMARY_TYPE, DESCRIPTION, LOCATION_DESCRIPTION \
FROM CHICAGO_CRIME_DATA \
WHERE (LOCATION_DESCRIPTION LIKE '%SCHOOL%');
```

# Problem 6  

## List the type of schools along with the average safety score for each type.  

