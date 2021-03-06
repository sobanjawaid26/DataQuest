
## <font color=blue>01 Introduction to the Data</font>
### <font color=black>Import pandas and read the CSV file academy_awards.csv into a Dataframe using the read_csv method.</font>
-  <font color=black>When reading the CSV, make sure to set the encoding to ISO-8859-1 so it can be parsed properly.</font>

### <font color=black>Start exploring the data in Pandas and look for data quality issues.</font>
-  <font color=black>Use the head method to explore the first few rows in the Dataframe.</font>
-  <font color=black>There are 6 unnamed columns at the end. Use the value_counts method to explore if any of them have valid values that we need.</font>
-  <font color=black>You'll notice that the Additional Info column contains a few different formatting styles. Start brainstorming ways to clean this column up.</font>


```python
import pandas as pd

academy_awards = pd.read_csv('academy_awards.csv',encoding='ISO-8859-1')
```


```python
print('\nNumber of rows:',  academy_awards.shape[0])
print('Number of cols:', academy_awards.shape[1])
academy_awards.head(3)
```

    
    Number of rows: 10137
    Number of cols: 11
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Nominee</th>
      <th>Additional Info</th>
      <th>Won?</th>
      <th>Unnamed: 5</th>
      <th>Unnamed: 6</th>
      <th>Unnamed: 7</th>
      <th>Unnamed: 8</th>
      <th>Unnamed: 9</th>
      <th>Unnamed: 10</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2010 (83rd)</td>
      <td>Actor -- Leading Role</td>
      <td>Javier Bardem</td>
      <td>Biutiful {'Uxbal'}</td>
      <td>NO</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2010 (83rd)</td>
      <td>Actor -- Leading Role</td>
      <td>Jeff Bridges</td>
      <td>True Grit {'Rooster Cogburn'}</td>
      <td>NO</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010 (83rd)</td>
      <td>Actor -- Leading Role</td>
      <td>Jesse Eisenberg</td>
      <td>The Social Network {'Mark Zuckerberg'}</td>
      <td>NO</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
print('Checking unnamed columns for useful information...\n')

print('-' * 75)
for i in range(5,11):
    col = 'Unnamed: %d' % i
    print(col)
    print(academy_awards[col].value_counts())
    print('-' * 75 + '\n')
```

    Checking unnamed columns for useful information...
    
    ---------------------------------------------------------------------------
    Unnamed: 5
    *                                                                                                               7
     D.B. "Don" Keele and Mark E. Engebretson has resulted in the over 20-year dominance of constant-directivity    1
     resilience                                                                                                     1
     error-prone measurements on sets. [Digital Imaging Technology]"                                                1
     discoverer of stars                                                                                            1
    Name: Unnamed: 5, dtype: int64
    ---------------------------------------------------------------------------
    
    Unnamed: 6
    *                                                                   9
     flexibility and water resistance                                   1
     direct radiator bass style cinema loudspeaker systems. [Sound]"    1
     sympathetic                                                        1
    Name: Unnamed: 6, dtype: int64
    ---------------------------------------------------------------------------
    
    Unnamed: 7
     while requiring no dangerous solvents. [Systems]"    1
    *                                                     1
     kindly                                               1
    Name: Unnamed: 7, dtype: int64
    ---------------------------------------------------------------------------
    
    Unnamed: 8
    *                                                 1
     understanding comedy genius - Mack Sennett.""    1
    Name: Unnamed: 8, dtype: int64
    ---------------------------------------------------------------------------
    
    Unnamed: 9
    *    1
    Name: Unnamed: 9, dtype: int64
    ---------------------------------------------------------------------------
    
    Unnamed: 10
    *    1
    Name: Unnamed: 10, dtype: int64
    ---------------------------------------------------------------------------
    
    

## <font color=blue>02 Filtering the Data</font>
### <font color=black>Before we filter the data, let's clean up the Year column by selecting just the first 4 digits in each value in the column, therefore excluding the value in parentheses:</font>
-  <font color=black>Use Pandas vectorized string methods to select just the first 4 elements in each string. (E.g. df["Year"].str[0:2] returns a Series containing just the first 2 characters for each element in the Year column.)</font>
-  <font color=black>Assign this new Series to the Year column to overwrite the original column.</font>
-  <font color=black>Convert the Year column to the int64 data type using astype. Make sure to reassign the integer Series object back to the Year column in the Dataframe or the changes won't be reflected.</font>

### <font color=black>Use conditional filtering to select only the rows from the Dataframe where the Year column is larger than 2000. Assign the new filtered Dataframe to later_than_2000.</font>

### <font color=black>Use conditional filtering to select only the rows from later_than_2000 where the Category matches one of the 4 awards we're interested in.</font>

##### <font color=black>Create a list of strings named award_categories with the following strings:</font>
-  <font color=black>Actor -- Leading Role</font>
-  <font color=black>Actor -- Supporting Role</font>
-  <font color=black>Actress -- Leading Role</font>
-  <font color=black>Actress -- Supporting Role</font>

##### <font color=black>Use the isin method in the conditional filter to return all rows in a column that match any of the values in a list of strings.</font>
-  <font color=black>Pass in award_categories to the isin method to return all rows : later_than_2000[later_than_2000["Category"].isin(award_categories)]</font>
-  <font color=black>Assign the resulting Dataframe to nominations.</font>


```python
academy_awards['Year'] = academy_awards['Year'].str[0:4].astype('int64')

later_than_2000 = academy_awards[academy_awards['Year'] > 2000]

award_categories = ['Actor -- Leading Role','Actor -- Supporting Role','Actress -- Leading Role','Actress -- Supporting Role']
nominations = later_than_2000[later_than_2000['Category'].isin(award_categories)]
nominations.head(3)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Nominee</th>
      <th>Additional Info</th>
      <th>Won?</th>
      <th>Unnamed: 5</th>
      <th>Unnamed: 6</th>
      <th>Unnamed: 7</th>
      <th>Unnamed: 8</th>
      <th>Unnamed: 9</th>
      <th>Unnamed: 10</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2010</td>
      <td>Actor -- Leading Role</td>
      <td>Javier Bardem</td>
      <td>Biutiful {'Uxbal'}</td>
      <td>NO</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2010</td>
      <td>Actor -- Leading Role</td>
      <td>Jeff Bridges</td>
      <td>True Grit {'Rooster Cogburn'}</td>
      <td>NO</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010</td>
      <td>Actor -- Leading Role</td>
      <td>Jesse Eisenberg</td>
      <td>The Social Network {'Mark Zuckerberg'}</td>
      <td>NO</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## <font color=blue>03 Cleaning up the Won? & Unnamed columns</font>
### <font color=black>Use the Series method map to replace all NO values with 0 and all YES values with 1.</font>
##### <font color=black>Select the Won? column from nominations.</font>
##### <font color=black>Then create a dictionary where each key is a value we want to replace and each value is the corresponding replacement value.</font>
-  <font color=black>The following dictionary replace_dict = { True: 1, False: 0 } would replace all True values with 1 and all False values with 0.</font>

##### <font color=black>Call the map function on the Series object and pass in the dictionary you created.</font>
##### <font color=black>Finally, reassign the new Series object to the Won? column in nominations.</font>

### <font color=black>Create a new column Won that contains the values from the Won? column.</font>
##### <font color=black>Select the Won? column and assign it to the Won column. Both columns should be in the Dataframe still.</font>

### <font color=black>Use the drop method to remove the extraneous columns.</font>
##### <font color=black>As the required parameter, pass in a list of strings containing the following values:</font>
-  <font color=black>Won?</font>
-  <font color=black>Unnamed: 5</font>
-  <font color=black>Unnamed: 6</font>
-  <font color=black>Unnamed: 7</font>
-  <font color=black>Unnamed: 8</font>
-  <font color=black>Unnamed: 9</font>
-  <font color=black>Unnamed: 10</font>

##### <font color=black>Set the axis parameter to 1 when calling the drop method</font>
##### <font color=black>Assign the resulting Dataframe to final_nominations.</font>


```python
nominations['Won'] = nominations['Won?'].map({'YES': 1, 'NO': 0})

remove_cols = ["Unnamed: {}".format(i) for i in range(5, 11)]
remove_cols.append('Won?')
final_nominations = nominations.drop(remove_cols, axis=1)
final_nominations.head(3)
```

    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """Entry point for launching an IPython kernel.
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Nominee</th>
      <th>Additional Info</th>
      <th>Won</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2010</td>
      <td>Actor -- Leading Role</td>
      <td>Javier Bardem</td>
      <td>Biutiful {'Uxbal'}</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2010</td>
      <td>Actor -- Leading Role</td>
      <td>Jeff Bridges</td>
      <td>True Grit {'Rooster Cogburn'}</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010</td>
      <td>Actor -- Leading Role</td>
      <td>Jesse Eisenberg</td>
      <td>The Social Network {'Mark Zuckerberg'}</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## <font color=blue>04 Cleaning up the Addiitonal Info column</font>
##### <font color=black>Use vectorized string methods to clean up the Additional Info column:</font>
-  <font color=black>Select the Additional Info column and strip the single quote and closing brace ("'}") using the rstrip method. Assign the resulting Series object to additional_info_one.</font>
-  <font color=black>Split additional_info_one on the string, " {', using the split method and assign to additional_info_two. Each value in this Series object should be a list containing the movie name first then the character name.</font>
-  <font color=black>Access the first element from each list in additional_info_two using vectorized string methods and assign to movie_names. Here's what the code looks like: additional_info_two.str[0]</font>
-  <font color=black>Access the second element from each list in additional_info_two using vectorized string methods and assign to characters.</font>

##### <font color=black>Assign the Series movie_names to the Movie column in the final_nominations Dataframe.</font>
##### <font color=black>Assign the Series characters to the Character column in the final_nominations Dataframe.</font>
##### <font color=black>Use the head method to preview the first few rows to make sure the values in the Character and Movie columns resemble the Additional Info column.</font>
##### <font color=black>Drop the Additional Info column using the drop method.</font>
##### <font color=black>Your Dataframe should look like:</font>

|  | Year | Category | Nominee | Won | Movie | Characer |
| :----- | :----- | :----- | :----- | :----- | :----- | :----- |
|0|2010|Actor -- Leading Role|Javier Bardem|0|Buitiful|Uxbal|
|1|2010|Actor -- Leading Role|Jeff Bridges|0|True Grit|Rooster Cogburn|


```python
additional_info_one = final_nominations['Additional Info'].str.rstrip("'}")
additional_info_two = additional_info_one.str.split(" {'")
final_nominations['Movie'] = additional_info_two.str[0]
final_nominations['Character'] = additional_info_two.str[1]
final_nominations.drop(['Additional Info'], axis=1, inplace=True)
final_nominations.head(3)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Nominee</th>
      <th>Won</th>
      <th>Movie</th>
      <th>Character</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2010</td>
      <td>Actor -- Leading Role</td>
      <td>Javier Bardem</td>
      <td>0</td>
      <td>Biutiful</td>
      <td>Uxbal</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2010</td>
      <td>Actor -- Leading Role</td>
      <td>Jeff Bridges</td>
      <td>0</td>
      <td>True Grit</td>
      <td>Rooster Cogburn</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010</td>
      <td>Actor -- Leading Role</td>
      <td>Jesse Eisenberg</td>
      <td>0</td>
      <td>The Social Network</td>
      <td>Mark Zuckerberg</td>
    </tr>
  </tbody>
</table>
</div>



## <font color=blue>05 Exporing to SQLite</font>
### <font color=black>Create the SQLite database nominations.db and connect to it.</font>
##### <font color=black>Import sqlite3 into the environment.</font>
##### <font color=black>Use the sqlite3 method connect to connect to the database file nominations.db.</font>
-  <font color=black>Since it doesn't exist in our current directory, it will be automatically created.</font>
-  <font color=black>Assign the returned Connection instance to conn.</font>

### <font color=black>Use the Dataframe method to_sql to export final_nominations to nominations.db.</font>
##### <font color=black>For the first parameter, set the table name to "nominations".</font>
##### <font color=black>For the second parameter, pass in the Connection instance.</font>
##### <font color=black>Set the index parameter to False.</font>


```python
import sqlite3
conn = sqlite3.connect('nominations.db')

final_nominations.to_sql(name='nominations' ,con=conn ,index=False)
```

## <font color=blue>06 Verifying in SQL</font>
### <font color=black>Import sqlite3 into the environment.</font>
### <font color=black>Create a Connection instance using the sqlite3 method connect to connect to your database file.</font>
### <font color=black>Explore the database to make sure the nominations table matches our Dataframe.</font>
##### <font color=black>Return and print the schema using pragma table_info(). The following schema should be returned:</font>
-  <font color=black>Year: Integer</font>
-  <font color=black>Category: Text</font>
-  <font color=black>Nominee: Text</font>
-  <font color=black>Won: Text</font>
-  <font color=black>Movie: Text</font>
-  <font color=black>Character: Text</font>

##### <font color=black>Return and print the first 10 rows using the SELECT and LIMIT statements.</font>
### <font color=black>Once you're done, use the Connection method close to close the connection to the database.</font>


```python
q1 = '''PRAGMA table_info(nominations);'''

nom = conn.execute(q1).fetchall()
for n in nom:
    print(n)
```

    (0, 'Year', 'INTEGER', 0, None, 0)
    (1, 'Category', 'TEXT', 0, None, 0)
    (2, 'Nominee', 'TEXT', 0, None, 0)
    (3, 'Won', 'INTEGER', 0, None, 0)
    (4, 'Movie', 'TEXT', 0, None, 0)
    (5, 'Character', 'TEXT', 0, None, 0)
    


```python
q2 = '''SELECT * FROM nominations LIMIT 10;'''
nom_peak = pd.read_sql_query(q2, conn)
nom_peak.head(3)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Nominee</th>
      <th>Won</th>
      <th>Movie</th>
      <th>Character</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2010</td>
      <td>Actor -- Leading Role</td>
      <td>Javier Bardem</td>
      <td>0</td>
      <td>Biutiful</td>
      <td>Uxbal</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2010</td>
      <td>Actor -- Leading Role</td>
      <td>Jeff Bridges</td>
      <td>0</td>
      <td>True Grit</td>
      <td>Rooster Cogburn</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010</td>
      <td>Actor -- Leading Role</td>
      <td>Jesse Eisenberg</td>
      <td>0</td>
      <td>The Social Network</td>
      <td>Mark Zuckerberg</td>
    </tr>
  </tbody>
</table>
</div>




```python
conn.close()
```
