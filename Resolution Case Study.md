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

## Chicago Crime Data  

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

