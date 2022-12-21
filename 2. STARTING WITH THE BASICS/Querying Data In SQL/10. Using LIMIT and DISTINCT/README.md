# Using LIMIT

We can limit the number of records in the results by providing a specific number of records to the `LIMIT` keyword. It is an optional keyword and is used after the `SELECT` keyword in the following form :

```SQL
SELECT [COLUMNS_NAMES]
FROM [TABLE_NAME]
LIMIT [n];
```

- **[n]** : is the number of records you want to retrieve

It is important to know that the `LIMIT` statement limits the number of records from the top row, working its way downward. It will execute based on the condition mentioned in the query. 

This means that when the query is executed, the clauses with conditions(the `WHERE` and `ORDER BY` clauses) will be applied first, and then the top n rows will be retrieved.

## Using the LIMIT Keyword

The store manger want to know what are the five most expensive items in the product catalog. To find this information we will need to do the following :

```SQL
SELECT ProductName, NetRetailPrice
FROM Products
ORDER BY NetRetailPrice DESC
LIMIT 5;
```

The result is as follows :

![Alt text](Images/limit%205%20query.png)

As you can see, the result table is showing only the top five records after the result has been ordered in a descending order.

If we used the same query and changed the number to 4 instead of 5, the query will look like this :

```SQL
SELECT ProductName, NetRetailPrice
FROM Products
ORDER BY NetRetailPrice DESC
LIMIT 4;
```

![Alt text](Images/only%204%20limit.png)

# Using DISTINCT

If we want to make sure we get distinct records (records with no duplication), we can use the `DISTINCT` keyword. 
The `DISTINCT` is an optional keyword that is used after the `SELECT` keyword in the following form :

```SQL
SELECT DISTINCT [COLUMNS_NAMES]
FROM [TABLE_NAME]
```

The `DISTINCT` keyword is commonly used with individual columns to make sure the retrieved column has unique values. 

Lets see an example on that, but first let's set up a demo database. Run the 'Customers1.sql' script (You Can Find It HERE). It will create a new table called `Customers1` that contains duplicate values.

You can see its contents by running the following query :

```SQL
SELECT * FROM Customers1
```

The result is as follows :

![Alt text](Images/duplicated%20rows.png)

If you take a closer look at the result you will see that the last 2 rows have the same first name 'Stephen' and also rows 2 and 7 are both 'Grace'. Let's see what will happen once we add the `DISTINCT` keyword to the statement :

```SQL
SELECT DISTINCT FirstName, LastName
FROM Customers1;
```

The result is as follows :

![Alt text](Images/nothing%20changed.png)

We see that we get back the same number of records because there are no two rows that are identical across all the selected columns (`FirstName` and `LastName`).

Let's use the same query with the `LastName` column **removed** and see what happens :

```SQL
SELECT DISTINCT FirstName
FROM Customers1;
```

![Alt text](Images/duplicates%20removes.png)

We can see that there are only seven records. The duplicate **Grace** and **Stephen** entries have not been included in the results.