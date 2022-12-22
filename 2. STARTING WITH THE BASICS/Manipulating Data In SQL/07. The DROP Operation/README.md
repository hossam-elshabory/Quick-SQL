# The `DROP` Operation

We will now look at the `DROP` operation, to see how we can delete the schema. The syntax is as follows:

The DROP operation is used to delete tables and schemas with the following syntax

```SQL
DROP TABLE <table_name>;
```

If we want to drop the `Customers` table in the `my_online_store` database, we will do it like this (Just an example, DO NOT FOLLOW ALONG!):

```SQL
DROP TABLE Customers
```

# Dropping Tables

Let's create a table and call it `drop_demo` to see how the `DROP` operation work in action.

1. To Create the table just like we did in the [Creating Databases and Tables]() section we will use this statement :

```SQL
-- Drops the table if already existing in the database
DROP TABLE IF EXISTS drop_demo;  

-- Creates the table with columns
CREATE TABLE drop_demo (  
	demoID       INT PRIMARY KEY AUTO_INCREMENT,  
	demoName     VARCHAR(20) ,  
	demoCountry  VARCHAR(50),
    demoAge 	 INT
);
```

2. Lets also add some data to it :

```SQL
INSERT INTO drop_demo (
	demoName,
    demoCountry,
    demoAge
)
VALUES 
	('mohamed', 'egypt', 22 ),
    ('Ahmed', 'egypt', 23),
    ('Sara', 'Egypt', 24);
```

3. If we check out the `drop_demo` table :

![Alt text](Images/demodrop%20table.png)

We can also see it in the Navigator Pane :

![Alt text](Images/drop%20demo%20in%20nav%20pane.png)

1. Now, let's drop it :

```SQL
DROP TABLE drop_demo;
```

5. The `demo_drop` is now gone 

![Alt text](Images/demo%20table%20is%20gone.png)