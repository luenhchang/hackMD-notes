###### tags: `python` `bash` `PBS` `array.array()` `len()` `DataFrame.fillna()` `DataFrame.replace()` `DataFrame.rename()` `DataFrame.isin()` `DataFrame.drop_duplicates()` `DataFrame.sort_values()` `pd.DataFrame()` `DataFrame.info()` `DataFrame.isnull()` `DataFrame.notna()` `DataFrame.to_csv()` `DataFrame['column'].unique()` `numpy.c_` `numpy.r_` `from` `import` `DataFrame.values` `flatten()` `tuple()` `reduce()` `DataFrame.iloc()` `__init__()` `__name__` `+` `lambda` `sum()`

# Data manipulation in Python

---

## Application Profram Interface (API)

What is an API? An API is a way of returning data by sending a HTTP or GET request to a website or server and it will send you a response back with data. [Dash and python 2: Dash core components](https://youtu.be/NM8Ue4znLP8)

---

## Run Python files
### Run a Python script on the cluster
[Call for a python script from a PBS job](https://stackoverflow.com/questions/45609240/call-for-a-python-script-from-a-pbs-job)
```bash!
#!/bin/bash
#PBS -N # job name 
#PBS -q 
#PBS -l 
#PBS -S /bin/bash
#PBS -m 
#PBS -M #my email 
cd $PBS_O_WORKDIR
python3 my_python_script.py
```

Adrian Campos's example of calling a python script from a PBS job 
```bash!
#!/bin/bash
#PBS -N PRSsubmit
#PBS -r n
#PBS -l mem=32GB,walltime=10:00:00
module load python
cd $PBS_O_WORKDIR #must qsub from working directory!
jobName="prsSNRr10"
runPRS ../Snoring_QC_results.tsv ${jobName} -std -r10
echo "Finished Running PRS"
```

### Run a python file on the terminal
![](https://i.imgur.com/EiLkLEw.png)


---

## pip.exe
`pip` is a python dependency manager.

Syntax `pip freeze` shows python packages that have been installed
Syntax `pip install -r D:\googleDrive\DataCamp_Dash-for-beginners\python-getting-started\requirements.txt` will install packages and versions specified in the requirements.txt file
Syntax `pip list` lists all the installed python packages. Note that if multiple python script folders (e.g., `D:\Anaconda3\Scripts` and `D:\python_3.8.1\Scripts`) have been added to the Path environmental variable, this will list the installed packages under the first directory. This was changed later to `D:\python_3.8.1\Scripts;D:\Anaconda3;D:\Anaconda3\Scripts;D:\Anaconda3\Library\bin`


---

## Python module
A module is a single .py file (or files) that are imported under one import and used. 

`import aModuleName` Here 'aModuleName' is just a regular .py file.

[Python3 importlib.util.spec_from_file_location with relative path?](https://stackoverflow.com/questions/48841150/python3-importlib-util-spec-from-file-location-with-relative-path)
* `import A` import a Python module called A
* `import A as a` import a Python module called A and abbreviate this module as 'a'
* `from B import c` import a function 'c' from the module 'B'
[Use 'import module' or 'from module import'?](https://stackoverflow.com/questions/710551/use-import-module-or-from-module-import)
[Creating and Importing Modules in Python](https://stackabuse.com/creating-and-importing-modules-in-python/)

---

### Import a module in a directory different from the current python script file
```python!
# A python file containing a function to be used in another python file
import itertools

def paste(*args, sep = ' ', collapse = None):
    """
    Port of paste from R
    Args:
        *args: lists to be combined
        sep: a string to separate the terms
        collapse: an optional string to separate the results
    Returns:
        A list of combined results or a string of combined results if collapse is not None
    """
    combs = list(itertools.product(*args))
    out = [sep.join(str(j) for j in i) for i in combs]
    if collapse is not None:
        out = collapse.join(out)
    return out

# Import the module above
import importlib.util
spec = importlib.util.spec_from_file_location(name='paste', location='D:/googleDrive/python/Python_functions/R-paste.py')
module = importlib.util.module_from_spec(spec)
spec.loader.exec_module(module)

# Call the function with moduleName.functionName()
module.paste(['s'], range(1,11), sep = '')
Out[307]: ['s1', 's2', 's3', 's4', 's5', 's6', 's7', 's8', 's9', 's10']

module.paste(['s'], range(2), ['t'], range(3), sep = '')
Out[63]: ['s0t0', 's0t1', 's0t2', 's1t0', 's1t1', 's1t2']

module.paste(['s'], range(2), ['t'], range(3), sep = '', collapse = ':')
Out[64]: 's0t0:s0t1:s0t2:s1t0:s1t1:s1t2'
```

---

## Python package
A package is a collection of modules in directories that give a package hierarchy. A package contains a distinct `__init__.py` file. `from aPackageName import aModuleName` Here 'aPackageName' is a folder with a `__init__.py` file and 'aModuleName', which is just a regular .py file. Therefore, the correct version of your proj-dir would be something like this,

 proj-dir
 --|--__init__.py
 --package1
 --|--__init__.py
 --|--module1.py
 --package2
 --|--__init__.py
 --|--module2.py

[Python3 importlib.util.spec_from_file_location with relative path?](https://stackoverflow.com/questions/48841150/python3-importlib-util-spec-from-file-location-with-relative-path)

---

### Dash
Dash is a web application framework that provides pure Python abstraction around HTML, CSS, and JavaScript. Instead of writing HTML or using an HTML templating engine, you compose your layout using Python structures with the dash-html-components library. The source for this library is on [GitHub](https://github.com/plotly/dash-html-components)

Here is an example of a simple HTML structure:
```python!
import dash_html_components as html

html.Div([
    html.H1('Hello Dash'),
    html.Div([
        html.P('Dash converts Python classes into HTML'),
        html.P('This conversion happens behind the scenes by Dash's JavaScript front-end')
    ])
])
```
which gets converted (behind the scenes) into the following HTML in your web-app:
```htmlmixed!
<div>
    <h1>Hello Dash</h1>
    <div>
        <p>Dash converts Python classes into HTML</p>
        <p>This conversion happens behind the scenes by Dash's JavaScript front-end</p>
    </div>
</div>
```
`<div> </div>` are meant to divide down your elements to structure your page, and other tags like sections, articles header...etc are just a kind of aliases of the div we know, and let's face it with their names they are making our life easier!

---

### gunicorn
The first few attempts to install the gunicorn package in cmd.exe with the following commands
```bash!
# First try
D:\Anaconda3\Scripts>pip install gunicorn
# Second try
D:\Anaconda3\Scripts>pip --trusted-host pypi.python.org --trusted-host files.pythonhosted.org --trusted-host pypi.org install gunicorn
```
resulted in the following error:
`Could not fetch URL [need more reputation to post link]: There was a problem confirming the ssl certificate: HTTPSConnectionPool(host='pypi.org (http://pypi.org)', port=443): Max retries exceeded with url: /simple/pip/ (Caused by SSLError("Can't connect to HTTPS URL because the SSL module is not avaiable.")) - skipping")`

Following the steps at [How do I set or change the PATH system variable?](https://www.java.com/en/download/help/path.xml (https://www.java.com/en/download/help/path.xml)) and [Requests (Caused by SSLError(“Can't connect to HTTPS URL because the SSL module is not available.”) Error in PyCharm requesting website (https://stackoverflow.com/questions/54135206/requests-caused-by-sslerrorcant-connect-to-https-url-because-the-ssl-module)](https://stackoverflow.com/questions/54135206/requests-caused-by-sslerrorcant-connect-to-https-url-because-the-ssl-module), this issue has been resolved by adding the following the to the Path environmental variable
`;D:\Anaconda3;D:\Anaconda3\Scripts;D:\Anaconda3\Library\bin;D:\python_3.8.1;D:\python_3.8.1\Scripts`

![](https://i.imgur.com/UYd48Zi.png)

---

## Jupyter Notebook
### Change default display in a Jupyter notebook
```python!
# Display all output in a cell
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"

# Widen the notebook with this code
from IPython.core.display import display, HTML
display(HTML("<style>.container { width:100% !important; }</style>"))
```

---
## Special Python variables

### `__file__`
`__file__` is the pathname of the file from which the module was loaded, if it was loaded from a file. This means `__file__` will only work when you run it as a script not in interpreter.(unless you import it in interpreter [what does the `__file__` variable mean/do?](https://stackoverflow.com/questions/9271464/what-does-the-file-variable-mean-do).

```python!
A = os.path.join(os.path.dirname(__file__), '..')
# A is the parent directory of the directory where program resides.

B = os.path.dirname(os.path.realpath(__file__))
# B is the canonicalised (?) directory where the program resides.

C = os.path.abspath(os.path.dirname(__file__))
# C is the absolute path of the directory where the program resides.

# Save the following code in a test.py at a different location, run the script and see the values
import os
print(__file__)
print(os.path.join(os.path.dirname(__file__), '..'))
print(os.path.dirname(os.path.realpath(__file__)))
print(os.path.abspath(os.path.dirname(__file__)))
```

---

### `__name__` 
`__name__` is a special Python variable. It gets its value depending on how we execute the containing script. When a script is running directory, the value of  `__name__` is `__main__`. When a script is running by importing as a module in another script, the value of  `__name__` is the name of the imported script.
[What’s in a (Python’s) `__name__`?](https://www.freecodecamp.org/news/whats-in-a-python-s-name-506262fe61e8/)
[What does `if __name__ == “__main__”:` do?](https://stackoverflow.com/questions/419163/what-does-if-name-main-do)

```python!
# If you are running your module (the source file) as the main program, e.g.
python foo.py
# It's as if the interpreter inserts this at the top of your module when run as the main program.
__name__ = "__main__" 

# Suppose this is in some other main program.
import foo
# It's as if the interpreter inserts this at the top of your module when it's imported from another module.
__name__ = "foo"

# file two.py
import one

print("top-level in two.py")
one.func()

if __name__ == "__main__":
    print("two.py is being run directly") # This is printed when two.py is running as the main program
else:
    print("two.py is being imported into another module") # This is printed when two.py is running as an imported module 
```

```python!
# This if statement ensures that app.run_server() is only executed when the script is running as a main program.
if __name__ == '__main__':
    app.run_server(debug=True)
```

---


## Data structure

### Array
#### Create an array
**array** is a collection of elements of the same type. 
Syntax `array.array(data_type, value_list)` creates an array of the same type specified by data_type. Note array.array() takes no keyword arguments.
[Python Arrays](https://www.geeksforgeeks.org/python-arrays/)

```python!
# Python program to demonstrate  
# Creation of Array  
  
# importing "array" for array creations 
import array as arr 
  
# creating an array with 3 integers
a = arr.array('i', [1, 2, 3]) 
  
# printing original array 
print ("The new created array is : ", end =" ") 
for i in range(0, 3): 
    print(a[i], end =" ") 
print() 
  
# creating an array with 3 floats (floats are double precision values in python)
b = arr.array('d', [2.5, 3.2, 3.3]) 
  
# printing original array 
print("The new created array is : ", end =" ") 
for i in range(0, 3): 
    print(b[i], end =" ") 
```

#### Concatenate arrays along row (first) or column axis
Syntax `numpy.r_[array1,array2,array3]` concatenates 3 arrays row-wise. The merged array grows in rows with column dimension unchanged
[numpy中np.c_和np.r_](https://blog.csdn.net/yj1556492839/article/details/79031693)

Syntax `numpy.c_[array1,array2,array3]` concatenates 3 arrays column-wise. The merged array grows in columns with row  dimension unchanged

Syntax `array.shape` shows the dimension of the array as (rows, columns)

```python!
import numpy as np
a = np.array([[1, 2, 3],[4,5,6]])
b = np.array([[0, 0, 0],[1,1,1]])
print("-------------------a------------------")
print(a)
print("The dimension of array a is: ", a.shape)
print("-------------------b------------------")
print(b)
print("The dimension of array b is: ", b.shape)
print("-------------------np.r_[a,b]--------------------")
print(np.r_[a,b])
print("The dimension of array np.r_[a,b] is: ", np.r_[a,b].shape)
print("--------------------np.c_[a,b]-------------------")
print(np.c_[a,b])
print("The dimension of array np.c_[a,b] is: ", np.c_[a,b].shape)

-------------------a------------------
[[1 2 3]
 [4 5 6]]
The dimension of array a is:  (2, 3)
-------------------b------------------
[[0 0 0]
 [1 1 1]]
The dimension of array b is:  (2, 3)
-------------------np.r_[a,b]--------------------
[[1 2 3]
 [4 5 6]
 [0 0 0]
 [1 1 1]]
The dimension of array np.r_[a,b] is:  (4, 3)
--------------------np.c_[a,b]-------------------
[[1 2 3 0 0 0]
 [4 5 6 1 1 1]]
The dimension of array np.c_[a,b] is:  (2, 6)
```

---

### Dictionary
Syntax `dictionary.keys()` shows all the keys
Syntax `dictionary.values()` shows all the values
Syntax `dictionary.items()` returns items in a list format of (key, value) tuple pairs

```python!
sammy = {'username': 'sammy-shark', 'online': True, 'followers': 987}
sammy.keys()
Out[599]: dict_keys(['username', 'online', 'followers'])

sammy.values()
Out[600]: dict_values(['sammy-shark', True, 987])

sammy.items()
Out[601]: dict_items([('username', 'sammy-shark'), ('online', True), ('followers', 987)])
```

---

### List

Syntax `list[0:10]` or `list[:10]` will give you the first 10 elements of this list using slicing
[Python: Fetch first 10 results from a list [duplicate]](https://stackoverflow.com/questions/10897339/python-fetch-first-10-results-from-a-list)
Syntax `list[-10:]` will give you the last 10 elements of this list using slicing
Syntax `len(list)` counts the number of items in the list

```python!
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
a[-9:]
[4, 5, 6, 7, 8, 9, 10, 11, 12]
```
---

### A List of Dictionaries

A list of dictionaries
[Book - 5.4) Python- Combining Lists and Dictionaries](https://canvas.instructure.com/courses/1133362/pages/book-5-dot-4-python-combining-lists-and-dictionaries)

```python!
Blacksburg_Forecast = [ { 'humidity' :  20, 'temperature' : 78, 'wind' :  7} ,
                        { 'humidity' :  50, 'temperature' : 61, 'wind' : 10} ,
                        { 'humidity' : 100, 'temperature' : 81, 'wind' :  5} ,
                        { 'humidity' :  90, 'temperature' : 62, 'wind' : 15} ,
                        { 'humidity' :  30, 'temperature' : 84, 'wind' : 19} ,
                        { 'humidity' :   0, 'temperature' : 66, 'wind' : 28} ,
                        { 'humidity' :   0, 'temperature' : 87, 'wind' : 12} ,
                        { 'humidity' :   0, 'temperature' : 68, 'wind' : 14} ,
                        { 'humidity' :   0, 'temperature' : 86, 'wind' :  4} ,
                        { 'humidity' :  60, 'temperature' : 68, 'wind' :  0}
                       ]
```
is graphically like a table, where each key

![](https://i.imgur.com/Zwu1JKc.png)

Each row in the list is an instance (i.e. dictionary)

```python!
# Iterate through each instance of the list and print the values of key wind
for forecast in Blacksburg_Forecast: 
     wind_speed = forecast['wind']  
     print(wind_speed) 

7
10
5
15
19
28
12
14
4
0
```

---

### String

---

## Manipulating a DataFrame (df)

### Read a CSV file from Google drive
Change the sharing settings of the CSV file to public. Who has access : Anyone who has the link can view

The Link to share is like `https://drive.google.com/file/d/1oNrjNmIF42SfIUzki-iyXnR04FJ2e4Jy/view?usp=sharing`, which ends with `usp=sharing`

Replace the link to share with the value of `orig_url` in the following code

[Pandas: How to read CSV file from google drive public?](https://stackoverflow.com/questions/56611698/pandas-how-to-read-csv-file-from-google-drive-public)

```python!
import pandas as pd
import requests
from io import StringIO

orig_url='https://drive.google.com/file/d/1oNrjNmIF42SfIUzki-iyXnR04FJ2e4Jy/view?usp=sharing'

file_id = orig_url.split('/')[-2]
dwn_url='https://drive.google.com/uc?export=download&id=' + file_id
url = requests.get(dwn_url).text
csv_raw = StringIO(url)
dfs = pd.read_csv(csv_raw)
print(dfs.head())

#   Study ID Study Hospital Study Group    variable  value
#0         1              A         TMT  Heart Rate  182.0
#1         2              A         TMT  Heart Rate  132.0
#2         3              A         TMT  Heart Rate  132.0
#3         4              A        CTRL  Heart Rate  132.0
#4         5              A         TMT  Heart Rate  138.0
```

---

### List column names
Syntax `DataFrame.columns.values`

---

### Add header to columns
Syntax `DataFrame.columns= ['NewColumn1','NewColumn2','NewColumn3']` adds header rows to a 3 column DataFrame

[How to add header row to a pandas DataFrame](https://stackoverflow.com/questions/34091877/how-to-add-header-row-to-a-pandas-dataframe)

```python!
    GDP= pd.read_excel('D:/Now/workshops/20191013_coursera_Introduction-to-Data-Science-in-Python/course1_downloads/gdplev.xls'
                      ,header=None
                      ,skiprows=8)
    # Select 5th and 7th columns
    t2= GDP.iloc[:,[4,6]]
    # Add header 
    t2.columns= ['YearQuarter','GDP']
```

---

### Rename specific columns
syntax `DataFrame.rename(columns={'oldName1': 'newName1', 'oldName2': 'newName2',...})`
[Renaming columns in pandas](https://stackoverflow.com/questions/11346283/renaming-columns-in-pandas)

```python!
# Rename column 'Energy Supply (petajoule)' as 'Energy Supply'
df= df.rename(columns={'Energy Supply (petajoule)':'Energy Supply'})
```

---

### Change data types
`DataFrame["A"]= pd.to_numeric(DataFrame["A"])` change the A column to numeric

```python!
# Check data types
data2.dtypes
#study_id            int64
#study_hospital     object
#study_group        object
#weight_kg         float64
#pathology          object
#hr                 object
#rr                 object
#spo2              float64

# Change data type for continuous variables to numeric
data2["hr"]=pd.to_numeric(data2["hr"])
data2["rr"]=pd.to_numeric(data2["rr"])

# Check data types again
data2.info()
#<class 'pandas.core.frame.DataFrame'>
#RangeIndex: 300 entries, 0 to 299
#Data columns (total 8 columns):
#study_id          300 non-null int64
#study_hospital    300 non-null object
#study_group       300 non-null object
#weight_kg         300 non-null float64
#pathology         296 non-null object
#hr                296 non-null float64
#rr                296 non-null float64
#spo2              298 non-null float64
#dtypes: float64(4), int64(1), object(3)
```
---

### Subset columns by a list
Syntax `DataFrame[['column_1','column_2','column_3']]`
[Selecting multiple columns in a pandas dataframe](https://stackoverflow.com/questions/11285613/selecting-multiple-columns-in-a-pandas-dataframe)

---

### Subset columns by positions
Syntax `DataFrame.loc[:,np.r_[0,n1:n2]]` selects all rows and column first, n1+1 ~ n2-1
[Selecting multiple dataframe columns by position in pandas [duplicate]](https://stackoverflow.com/questions/48545076/selecting-multiple-dataframe-columns-by-position-in-pandas)

```python!
import numpy as np
# Select all rows and column first, 51st ~ 59th
GDP2006_2015=GDP.iloc[:, np.r_[0,50:60]] # len(GDP2006_2015) 264
```

---

### Combine multiple columns into a new column
`DataFrame['NewColumn']= DataFrame['string_column1'] + DataFrame['string_column2']`

`DataFrame['NewColumn']= DataFrame['string_column1'] + " " +DataFrame['string_column2']`

`DataFrame['NewColumn']= DataFrame['numeric_column1'].apply(str) + DataFrame['numeric_column2'].apply(str)` convert the column to string using `.apply(str)` before the concatenation

```python!
import pandas as pd
# create a new data frame
df = pd.DataFrame({'Last': ['Smith', 'Nadal', 'Federer'],
                   'First': ['Steve', 'Joe', 'Roger'],
                   'Age':[32,34,36]})
df["Name"] = df["First"] +" "+ df["Last"]

df
Out[316]: 
      Last  First  Age           Name
0    Smith  Steve   32    Steve Smith
1    Nadal    Joe   34      Joe Nadal
2  Federer  Roger   36  Roger Federer
```
---


### Get unique rows
Syntax `DataFrame.drop_duplicates()`
[How to “select distinct” across multiple data frame columns in pandas?](https://stackoverflow.com/questions/30530663/how-to-select-distinct-across-multiple-data-frame-columns-in-pandas)

---

### Sort a df by multiple columns
Syntax `DataFrame_sorted= DataFrame.sort_values(['column1','column2'], ascending=[True, False])`
Syntax `DataFrame.sort_values(['column1','column2'], ascending=[True, False], inplace=True)`
[How to sort a dataFrame in python pandas by two or more columns?](https://stackoverflow.com/questions/17141558/how-to-sort-a-dataframe-in-python-pandas-by-two-or-more-columns)

---

### Filtering a df with and, or , not operators
Syntax: `DataFrame[(condition1) & (condition2) | (condition3) ~ (condition4)]` 

**&** means and
**|** means or
**~** means not

A condition can be specified as `DataFrame['column'] == ` or `DataFrame['column'] != `  
[Filtering Pandas Dataframe using OR statement](https://stackoverflow.com/questions/29461185/filtering-pandas-dataframe-using-or-statement)

---

### Filtering a df with in, not in operations

Syntax `DataFrame[(condition1) ~(condition2)]` filters the DataFrame with 1 positive condition and 1 negative condition. 

condition1 can be written as `DataFrame['column1'].isin(list1) == `
condition2 can be `DataFrame['column2'].isin(list2)`
[How to filter Pandas dataframe using 'in' and 'not in' like in SQL](https://stackoverflow.com/questions/19960077/how-to-filter-pandas-dataframe-using-in-and-not-in-like-in-sql)

---

### Filtering a df by selecting rows based on 1 column and select other columns

`df[df['column1']=='condition']['column2']` selects rows based on column1 and selects column2

```python!
# Folder locations
folder_path_project='D:/googleDrive/Udemy_Interactive-Python-Dashboards-with-Plotly-and-Dash/course_material/Plotly-Dashboards-with-Dash-master/'
folder_path_data= os.path.join(folder_path_project,'Data/')
folder_path_script=os.path.join(folder_path_project,'1-03E-LineChartExercises/')

# 
df= pd.read_csv(os.path.join(folder_path_data,'2010YumaAZ.csv'))

days = ['TUESDAY','WEDNESDAY','THURSDAY','FRIDAY','SATURDAY','SUNDAY','MONDAY']

# Use a for loop (or list comprehension to create traces for the data list)
data = []

for day in days:
    # Make a plot per day of week
    trace = go.Scatter(x=df['LST_TIME']
                      ,y=df[df['DAY']==day]['T_HR_AVG']
                      ,mode='lines'
                      ,name=day)
    # Add the current plot to the data list
    data.append(trace)

```

---


### Identifying missing and non-missing values
Syntax `DataFrame['column'].isnull().sum()` counts number of missing values in the column
Syntax `DataFrame['column'].notna().sum()` counts number of non-missing values in the column
Syntax `DataFrame.isnull().sum()` counts total NaN at each column in a DataFrame
[Python Pandas : Count NaN or missing values in DataFrame ( also row & column wise)](https://thispointer.com/python-pandas-count-number-of-nan-or-missing-values-in-dataframe-also-row-column-wise/)

```python!
# Count the number and percentage of missing values at each column
count_NaN_series= df.isnull().sum()
percent_NaN_series= count_NaN_series/df.shape[0]*100
# Create a DataFrame from Series data
frame={'NaN_count': count_NaN_series, 'NaN_percent':percent_NaN_series}
df_NaN= pd.DataFrame(frame)
```

---

### Replacing missing values
Syntax `DataFrame.fillna(value={'column1': value1, 'column2': value2},...)` replaces missing values in column1 with value1 and missing values in column2 with value2
[HOW TO USE THE PANDAS FILLNA METHOD](https://www.sharpsightlabs.com/blog/pandas-fillna/)

The replaced values can be `DataFrame['column'].mean()`, `DataFrame['column'].median()`, `DataFrame['column'].mode()`

`DataFrame.replace(to_replace = np.nan, value =-99999)` replaces NaN values with -99999

```python!
df_fillna =df.fillna(value={'qualification_edu_6138_zscore': df['qualification_edu_6138_zscore'].mean(),'complete_alcohol_unitsweekly':df['complete_alcohol_unitsweekly'].mean(),'caffeine.per.day': df['caffeine.per.day'].mean(),'age':df['age'].mean(),'overall_health_rating': df['overall_health_rating'].median(),'merged_pack_years_20161':df['merged_pack_years_20161'].mean(),'merged_pack_years_20161_zscore':df['merged_pack_years_20161_zscore'].mean(),'3456-0.0':df['3456-0.0'].mean()})
```

---

### Replacing string
`DataFrame.replace(to_replace="A", value="a")` replaces "A" with "a" across all columns

`DataFrame.replace(to_replace=["A","B"], value="ab")` replaces "A" and "B" with "a" across all columns

[Python | Pandas dataframe.replace()](https://www.geeksforgeeks.org/python-pandas-dataframe-replace/)

```python!
# Replace string "missing" with blank
data2= data.replace("missing","")
```

---

### Convert DataFrame to tuples
* [How to get a value from a Pandas DataFrame and not the index and object type](https://stackoverflow.com/questions/30787901/how-to-get-a-value-from-a-pandas-dataframe-and-not-the-index-and-object-type)
```python!
# Find the row with max value in % Renewable, return 2 columns
## The values attribute returns the values as a np array (e.g. [['Brazil' 69.64803]] ). It's a list of lists where
## each list element is from a row (e.g. [[row 1 values],[row 2 values]])
t6= t3.loc[t3['% Renewable']==t3['% Renewable'].max(), ['Country','% Renewable']].values
print(t6) # [['Brazil' 69.64803]]
# unlist the list of list
print(t6.flatten()) # ['Brazil' 69.64803]
# unlist the list of list, convert the list to a tuple
print(tuple(t6.flatten())) # ('Brazil', 69.64803)
```

---

### Subset rows based on conditions in 1 column
`DataFrame[condition]`

`DataFrame[(condition 1) | (condition 2)]`

`DataFrame[(condition 1) & (condition 2)]`

```python!
# Select rows from Rank=1 to Rank=15
ScimEn_rank1_15=ScimEn[ScimEn['Rank'] <= 15] # len(ScimEn_rank1_15) 15
```

---

### Reshpae data from wide to long

`pd.melt(DataFrame, id_vars=['g1','g2','g3'], value_vars=['m1','m2','m3'])` reshapes the data from wide to long format. The m1, m2, m3 columns are grouped into 2 new columns variable and value. The variable contains the name of m1, m2, m3 and the value column contains their values.

```python!
# Resetting index really needed before data reshape?
data = data.reset_index(level=0, drop=True).reset_index()

# Reshape 3 value_vars columns into 2 new columns- variable and value
data2 = pd.melt(data, id_vars=['Study ID','Study Hospital','Study Group'], value_vars=['Heart Rate','Respiratory Rate','spo2'])
```

---

### Join > 2 DataFrames
* [pandas three-way joining multiple dataframes on columns](https://stackoverflow.com/questions/23668427/pandas-three-way-joining-multiple-dataframes-on-columns/23671390)
* [Python pandas: Keep selected column as DataFrame instead of Series](https://stackoverflow.com/questions/16782323/python-pandas-keep-selected-column-as-dataframe-instead-of-series)

```python!
from functools import reduce

# Suppose you want to join single columns from 3 dataframes energy, GDP and ScimEn
## energy.shape (265, 5)
## GDP.shape (264, 60)
## ScimEn.shape (191, 8)

# Subset country columns from the 3 data sets
# Keep the column names identical for merging purposes
d1= energy.loc[:,['Country']]
d2= GDP.loc[:,['Country Name']]
d2= d2.rename(columns={'Country Name':'Country'})
d3= ScimEn.loc[:,['Country']]

# Compile the list of dataframes you want to merge
data_frames = [d1,d2,d3]

# Full join the 3 data sets
outer_join= reduce(lambda left, right: pd.merge(left, right, how="outer", on="Country"), data_frames)  
len(outer_join) # 356 

# Inner join the 3 data sets
inner_join= reduce(lambda left, right: pd.merge(left, right, how="inner", on="Country"), data_frames)  
len(inner_join) # 162

obs_diff_outer_inner= len(outer_join) - len(inner_join)
obs_diff_outer_inner # 194
```

---

### Convert DataFrame to dictionary
`mydict= dict(zip(DataFrame['columnA'], DataFrame['columnB']))` creates a new dictionary using columnA as key and columnB as value. [python pandas dataframe columns convert to dict key and value](https://stackoverflow.com/questions/7971618/python-return-first-n-keyvalue-pairs-from-dict)  

```python!
import os
import pandas as pd

# Folder locations
folder_path_project='D:/googleDrive/Udemy_Interactive-Python-Dashboards-with-Plotly-and-Dash/course_material/Plotly-Dashboards-with-Dash-master/'
folder_path_data=os.path.join(folder_path_project,'Data/')

# Download world cities from the Internet
world_cities= pd.read_csv(os.path.join(folder_path_data,'worldcities.csv'))
# Use the country column as key and iso2 column as value to create a new dictionary country_dict
country_dict=dict(zip(world_cities['country'], world_cities['iso2']))
```

---

### Export a DataFrame

`DataFrame.to_csv(path_or_buf='filepath',sep=',',na_rep='',header=True)` export a python object as a file
[pandas.DataFrame.to_csv](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_csv.html)

```python!
# Export cleaned data as a csv
data3.to_csv('D:/googleDrive/Job/UQ_509098_Data-analyst_Paediatric-Critical-Care-Research-Group/Practical-Assessment/cleaned_data_long.csv', index=False)
```
---

## List comprehension

```python!
# If you used to do it like this:
new_list = []
for i in old_list:
    if filter(i):
        new_list.append(expressions(i))

# You can obtain the same thing using list comprehension:
new_list = [expression(i) for i in old_list if filter(i)]
```

---

### Get column names with a list comprehension
```python!
list_of_pop_col= [col for col in df2.columns if col.startswith('POP')]
```

---

### Read all files in a folder and combine them to a single file with a list comprehension

```python!
# Change directory to the data folder
os.chdir('GWAS_calc/GWAS_out')
# Obtain file names that end with .dosage
dosageFiles=glob.glob('*.dosage')

# Print a message with current time
print("Compiling dosage results this may take a while %s"%(datetime.datetime.now()))

# Import all the .dosage files and concatenate them to a dataframe called finalDF
finalDF=pd.concat((pd.read_csv(f,header=0,index_col='SNP',sep='\s+') for f in dosageFiles))

# Print another message with current time
print("Finished compiling dosage at %s"%(datetime.datetime.now()))
```

---

### Make 1 plot per unique value of a column
```python!
# Perform imports here:
import pandas as pd
import os
import plotly.offline as pyo
import plotly.graph_objs as go

# Folder locations
folder_path_project='D:/googleDrive/Udemy_Interactive-Python-Dashboards-with-Plotly-and-Dash/course_material/Plotly-Dashboards-with-Dash-master/'
folder_path_data= os.path.join(folder_path_project,'Data/')
folder_path_script=os.path.join(folder_path_project,'1-03E-LineChartExercises/')

# Create a pandas DataFrame from 2010YumaAZ.csv
## Column LST_TIME and T_HR_AVG are used as x and y in the line plot
## Column DAY is used to make one plot per day of week
df= pd.read_csv(os.path.join(folder_path_data,'2010YumaAZ.csv'))
print(df.head())

days = ['TUESDAY','WEDNESDAY','THURSDAY','FRIDAY','SATURDAY','SUNDAY','MONDAY']

# Method 1: making 1 plot per week day using a for loop
data = []

for day in days:
    # Subset data from the day of week
    df2=df[df['DAY']==day]
    # Make a plot per day of week
    trace = go.Scatter(x=df2.LST_TIME
                      ,y=df2.T_HR_AVG
                      ,mode='lines'
                      ,name=day)
    # Add the current plot to the data list
    data.append(trace)

# Method 2: making 1 plot per week day using a list comprehension
data= [{
    'x': df['LST_TIME'],
    'y': df[df['DAY']==day]['T_HR_AVG'],
    'name': day
    } for day in df['DAY'].unique()]

# Define the layout
layout= go.Layout(title='Daily averaged temperature'
                ,xaxis=dict(title='Hours')
                ,yaxis=dict(title='Averaged temperature oC'))

#Pass plot data list and the layout variable to a go.Figure object
figu= go.Figure(data=data, layout=layout)

# Create a fig from data and layout, and plot the figures by passing the go.Figure object to a plot call. Export the scatter plot as a html file
output_file_path=os.path.join(folder_path_script,'line-plot_hourly-avg-temp.html')
pyo.plot(figu, filename=output_file_path)
```










