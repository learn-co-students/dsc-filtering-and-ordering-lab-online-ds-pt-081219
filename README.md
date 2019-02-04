
# Filtering and Ordering - Lab

For the old version of this lab go [here](https://github.com/learn-co-curriculum/dsc-1-05-08-filtering-and-ordering-lab-old).

## Introduction
In this lab, we will write more `SELECT` statements to solidify our ability to query a SQL database.  We will also write more specific queries using the tools we learned in the previous lesson.

## Objectives
You will be able to:
* Limit the number of records returned by a query using `LIMIT`
* Filter results using `BETWEEN` and `IS NULL`
* Order the results of your queries by using `ORDER BY` (`ASC` & `DESC`)

### Famous Dogs

We have a database full of famous dogs!  The `dogs` table is populated with the following data:

|name      |age    |gender |breed           |temperament|hungry |
|----------|-------|-------|----------------|-----------|-------|
|Snoopy    |3      |M      |beagle          |friendly   |1      |
|McGruff   |10     |M      |bloodhound      |aware      |0      |
|Scooby    |6      |M      |great dane      |hungry     |1      |
|Little Ann|5      |F      |coonhound       |loyal      |0      |
|Pickles   |13     |F      |black lab       |mischievous|1      |
|Clifford  |4      |M      |big red         |smiley     |1      |
|Lassie    |7      |F      |collie          |loving     |1      |
|Snowy     |8      |F      |fox terrier     |adventurous|0      |
|NULL      |4      |M      |golden retriever|playful    |1      |

## Connecting to the Database

First, import sqlite3 and establish a connection to the database **dogs.db**. Then, create a cursor object so that you can pass SQL queries to the database.


```python
#Your code here; import sqlite, create a connection and then a cursor object.
import sqlite3
conn = sqlite3.Connection('dogs.db')
cur = conn.cursor()
```

## Review

As a quick review, here's the code we used to generate this table:

```
cur.execute("""CREATE TABLE dogs (id INTEGER PRIMARY KEY,
                                   name TEXT, age INTEGER,
                                   gender CHAR(1),
                                   breed TEXT,
                                   temperament TEXT,
                                   hungry BOOLEAN);""")

cur.execute("""INSERT INTO dogs (name, age, gender, breed, temperament, hungry) VALUES
                ("Snoopy", 3, "M", "beagle", "friendly", 1),
                ("McGruff", 10, "M", "bloodhound", "aware", 0),
                ("Scooby", 6, "M", "great dane", "hungry", 1),
                ("Little Ann", 5, "F", "coonhound", "loyal", 0),
                ("Pickles", 13, "F", "black lab", "mischievous", 1),
                ("Clifford", 4, "M", "big red", "smiley", 1),
                ("Lassie", 7, "F", "collie", "loving", 1),
                ("Snowy", 8, "F", "fox terrier", "adventurous", 0),
                (NULL, 4, "M", "golden retriever", "playful", 1);""")
conn.commit() #Save our changes to the database
```

## Queries

Display the outputs for each of the following query descriptions.

* Select the name and breed for all female dogs


```python
#Your code here
cur.execute("""SELECT name, breed FROM dogs WHERE gender = 'F';""").fetchall()
```




    [('Little Ann', 'coonhound'),
     ('Pickles', 'black lab'),
     ('Lassie', 'collie'),
     ('Snowy', 'fox terrier')]



* Select the names of all dogs listed in alphabetical order.  Notice that SQL lists the nameless dog first.


```python
#Your code here
cur.execute("""SELECT name FROM dogs ORDER BY name;""").fetchall()
```




    [(None,),
     ('Clifford',),
     ('Lassie',),
     ('Little Ann',),
     ('McGruff',),
     ('Pickles',),
     ('Scooby',),
     ('Snoopy',),
     ('Snowy',)]



* Select any dog that doesn't have a name


```python
#Your code here
cur.execute("""SELECT * FROM dogs WHERE name IS NULL;""").fetchall()
```




    [(9, None, 4, 'M', 'golden retriever', 'playful', 1)]



* Select the name and breed of only the hungry dogs and lists them from youngest to oldest


```python
#Your code here
cur.execute("""SELECT name, breed FROM dogs WHERE hungry = 1 ORDER BY age;""").fetchall()
```




    [('Snoopy', 'beagle'),
     ('Clifford', 'big red'),
     (None, 'golden retriever'),
     ('Scooby', 'great dane'),
     ('Lassie', 'collie'),
     ('Pickles', 'black lab')]



* Select the oldest dog's name, age, and temperament


```python
#Your code here
cur.execute("""SELECT name, age, temperament FROM dogs ORDER BY age DESC LIMIT 1;""").fetchall()
```




    [('Pickles', 13, 'mischievous')]



* Select the three youngest dogs


```python
#Your code here
cur.execute("""SELECT name, age FROM dogs ORDER BY age LIMIT 3;""").fetchall()
```




    [('Snoopy', 3), ('Clifford', 4), (None, 4)]



* Select the name and breed of only the dogs who are between five and ten years old


```python
#Your code here
cur.execute("""SELECT name, breed FROM DOGS WHERE age BETWEEN 5 AND 10 ORDER BY age DESC;""").fetchall()
```




    [('McGruff', 'bloodhound'),
     ('Snowy', 'fox terrier'),
     ('Lassie', 'collie'),
     ('Scooby', 'great dane'),
     ('Little Ann', 'coonhound')]



* Select the name, age, and hungry columns for hungry dogs between the ages of two and seven.  This query should also list these dogs in alphabetical order.


```python
#Your code here
cur.execute("""SELECT name, age, hungry FROM dogs WHERE hungry = 1 AND age BETWEEN 2 AND 7 ORDER BY name;""").fetchall()
```




    [(None, 4, 1),
     ('Clifford', 4, 1),
     ('Lassie', 7, 1),
     ('Scooby', 6, 1),
     ('Snoopy', 3, 1)]



## Summary

Great work! In this lab we practiced writing more complex SQL statements to not only query specific information but also define the quantity of results and the order of our results. 
