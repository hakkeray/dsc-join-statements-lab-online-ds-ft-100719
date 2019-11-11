
# Join Statements - Lab

## Introduction

In this lab, you'll practice your knowledge of `JOIN` statements, using various types of joins and various methods for specifying the links between them.

## Objectives

You will be able to:
* Write SQL queries that make use of various types of joins
* Compare and contrast the various types of joins
* Discuss how primary and foreign keys are used in SQL
* Decide and perform whichever type of join is best for retrieving desired data

## CRM Schema

In almost all cases, rather than just working with a single table you will typically need data from multiple tables. 
Doing this requires the use of **joins** using shared columns from the two tables. 

In this lab, you'll use the same customer relationship management (CRM) database that you saw from the previous lesson.
<img src='images/Database-Schema.png' width="600">

## Connecting to the Database
Import the necessary packages and connect to the database **data.sqlite**.


```python
import pandas as pd
import sqlite3
conn = sqlite3.connect('data.sqlite')
cur = conn.cursor()
```

## Display the names of all the employees in Boston.
Hint: join the employees and offices tables.


```python
cur.execute("""SELECT firstName, lastName 
               FROM employees
               JOIN offices
               ON employees.officeCode = offices.officeCode
               WHERE city = 'Boston';
               """)
df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df.columns = [i[0] for i in cur.description]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Julie</td>
      <td>Firrelli</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Steve</td>
      <td>Patterson</td>
    </tr>
  </tbody>
</table>
</div>



## Are there any offices that have zero employees?
Hint: Combine the employees and offices tables and use a group by.


```python
cur.execute("""SELECT city, COUNT(*)
               FROM offices
               LEFT JOIN employees
               USING(officeCode)
               GROUP BY city;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>COUNT(*)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Boston</td>
      <td>2</td>
    </tr>
    <tr>
      <td>1</td>
      <td>London</td>
      <td>2</td>
    </tr>
    <tr>
      <td>2</td>
      <td>NYC</td>
      <td>2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Paris</td>
      <td>5</td>
    </tr>
    <tr>
      <td>4</td>
      <td>San Francisco</td>
      <td>6</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Sydney</td>
      <td>4</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Tokyo</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



## Write 3 Questions of your own and answer them


```python
# Answers will vary
# Example: Display the htmlDescription and employee's first and last name for each product that each employee has sold
```


```python
# calculate total sales dollars raked in by each employee
cur.execute("""SELECT firstName, lastName, SUM(amount)
               FROM employees e
               JOIN customers c                   
               ON e.employeeNumber = c.salesRepEmployeeNumber
               JOIN payments p 
               USING(customerNumber)
               GROUP BY lastName
               ORDER BY firstName""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
print(len(df))
df
```

    15





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>SUM(amount)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Andy</td>
      <td>Fixter</td>
      <td>509385.82</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Barry</td>
      <td>Jones</td>
      <td>637672.65</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Foon Yue</td>
      <td>Tseng</td>
      <td>488212.67</td>
    </tr>
    <tr>
      <td>3</td>
      <td>George</td>
      <td>Vanauf</td>
      <td>584406.80</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Gerard</td>
      <td>Hernandez</td>
      <td>1112003.81</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Julie</td>
      <td>Firrelli</td>
      <td>386663.20</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Larry</td>
      <td>Bott</td>
      <td>686653.25</td>
    </tr>
    <tr>
      <td>7</td>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>989906.55</td>
    </tr>
    <tr>
      <td>8</td>
      <td>Leslie</td>
      <td>Thompson</td>
      <td>347533.03</td>
    </tr>
    <tr>
      <td>9</td>
      <td>Loui</td>
      <td>Bondur</td>
      <td>569485.75</td>
    </tr>
    <tr>
      <td>10</td>
      <td>Mami</td>
      <td>Nishi</td>
      <td>457110.07</td>
    </tr>
    <tr>
      <td>11</td>
      <td>Martin</td>
      <td>Gerard</td>
      <td>387477.47</td>
    </tr>
    <tr>
      <td>12</td>
      <td>Pamela</td>
      <td>Castillo</td>
      <td>750201.87</td>
    </tr>
    <tr>
      <td>13</td>
      <td>Peter</td>
      <td>Marsh</td>
      <td>497907.16</td>
    </tr>
    <tr>
      <td>14</td>
      <td>Steve</td>
      <td>Patterson</td>
      <td>449219.13</td>
    </tr>
  </tbody>
</table>
</div>




```python
# same thing but rank in order of most sales per office (based on city)
# calculate total sales dollars raked in by each employee
# include number of employees in that city/office

```

## Level Up: Display the names of every individual product that each employee has sold


```python
cur.execute("""SELECT firstName, lastName, productName
               FROM employees e
               JOIN customers c
               ON e.employeeNumber = c.salesRepEmployeeNumber
               JOIN orders o
               USING(customerNumber)
               JOIN orderdetails od
               USING(orderNumber)
               JOIN products p
               USING(productCode)""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
print(len(df))
df
```

    2996





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>productName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1958 Setra Bus</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1940 Ford Pickup Truck</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1939 Cadillac Limousine</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1996 Peterbilt 379 Stake Bed with Outrigger</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1968 Ford Mustang</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2991</td>
      <td>Martin</td>
      <td>Gerard</td>
      <td>1954 Greyhound Scenicruiser</td>
    </tr>
    <tr>
      <td>2992</td>
      <td>Martin</td>
      <td>Gerard</td>
      <td>1950's Chicago Surface Lines Streetcar</td>
    </tr>
    <tr>
      <td>2993</td>
      <td>Martin</td>
      <td>Gerard</td>
      <td>Diamond T620 Semi-Skirted Tanker</td>
    </tr>
    <tr>
      <td>2994</td>
      <td>Martin</td>
      <td>Gerard</td>
      <td>1911 Ford Town Car</td>
    </tr>
    <tr>
      <td>2995</td>
      <td>Martin</td>
      <td>Gerard</td>
      <td>1936 Mercedes Benz 500k Roadster</td>
    </tr>
  </tbody>
</table>
<p>2996 rows × 3 columns</p>
</div>



## Level Up: Display the Number of Products each employee has sold


```python
cur.execute("""SELECT firstName, lastName, COUNT(productName)
               FROM employees e
               JOIN customers c                   
               ON e.employeeNumber = c.salesRepEmployeeNumber
               JOIN orders o 
               USING(customerNumber)
               JOIN orderdetails od 
               USING(orderNumber)                    
               JOIN products p 
               USING(productCode)
               GROUP BY lastName
               ORDER BY firstName""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
print(len(df))
df
```

    15





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>COUNT(productName)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Andy</td>
      <td>Fixter</td>
      <td>185</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Barry</td>
      <td>Jones</td>
      <td>220</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Foon Yue</td>
      <td>Tseng</td>
      <td>142</td>
    </tr>
    <tr>
      <td>3</td>
      <td>George</td>
      <td>Vanauf</td>
      <td>211</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Gerard</td>
      <td>Hernandez</td>
      <td>396</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Julie</td>
      <td>Firrelli</td>
      <td>124</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Larry</td>
      <td>Bott</td>
      <td>236</td>
    </tr>
    <tr>
      <td>7</td>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>331</td>
    </tr>
    <tr>
      <td>8</td>
      <td>Leslie</td>
      <td>Thompson</td>
      <td>114</td>
    </tr>
    <tr>
      <td>9</td>
      <td>Loui</td>
      <td>Bondur</td>
      <td>177</td>
    </tr>
    <tr>
      <td>10</td>
      <td>Mami</td>
      <td>Nishi</td>
      <td>137</td>
    </tr>
    <tr>
      <td>11</td>
      <td>Martin</td>
      <td>Gerard</td>
      <td>114</td>
    </tr>
    <tr>
      <td>12</td>
      <td>Pamela</td>
      <td>Castillo</td>
      <td>272</td>
    </tr>
    <tr>
      <td>13</td>
      <td>Peter</td>
      <td>Marsh</td>
      <td>185</td>
    </tr>
    <tr>
      <td>14</td>
      <td>Steve</td>
      <td>Patterson</td>
      <td>152</td>
    </tr>
  </tbody>
</table>
</div>



## Summary

Congrats! You practiced using join statements and leveraged your foreign keys knowledge!
