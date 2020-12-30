# Advance-Database-Final-Exam-Project
# Database Source
This database is all about managing stocks and every stock category. Every stock has different name, description and category, and each category has its own name. Stock contains specific clothing types like Smockback dresses, Denim skirts and so on, while the categories tells whether a stock is a skirt, dress, pants and so on. Each stock has its own description that tells about the clothing’s color and design together with the topline that tells about where to wear it. All stock also differ from its price along with the photo containing each row of each stock.

![image](https://user-images.githubusercontent.com/73202856/103324269-c28a1d00-4a81-11eb-9f65-046df58f9b82.png)

Database source is from Phil Adams in his youtube video tutorials on how to create a dynamic site using PHP and mySQL. Here is the link of the video:
https://www.dropbox.com/sh/jbe5ai9wkn...

Stock Table – contains stockID, name, categoryID, price, thumbnail, bigphoto, topline and description attributes. StockID is the primary key which holds a unique key of every stock, name which holds the full name of the stock, categoryID is a foreign key that also holds a unique but from the other table, price which holds the sale price of the prodyct, thumbnail and bigphoto which holds pictures of the product, topline that suggests where to wore the product and lastly its every description that tells about its quality.

Category Table – contains the categoryID and the name. CategoryID is the primary key which holds the unique key of every category a stock belongs to and the name where it tells about the type of clothing of each product listed.

# Database Dependency Diagram

![image](https://user-images.githubusercontent.com/73202856/103324386-5d82f700-4a82-11eb-9e87-097c53def340.png)

Table 1 Stock attributes name, categoryID, price, thumbnail, bigphoto, topline and description is dependent on stock ID and Table 2 Category attirbute name is dependent on categoryID.

# Complex Queries
# Query 1 – SQL Partitions ( LIST Partitioning )

CREATE TABLE sale_mast2 (
     bill_no INT NOT NULL,
     bill_date TIMESTAMP NOT NULL,
     agent_codE INT NOT NULL,
     amount INT NOT NULL
) 
PARTITION  BY LIST(agent_code) (
     PARTITION pA VALUES IN (1,2,3), 
     PARTITION pB VALUES IN (4,5,6), 
     PARTITION pC VALUES IN (7,8,9,10,11
)
; 

- there are 11 agents that represents three cities, A, B and C, and it is arranged in three partitions with LIST Partitioning.

IMPORTANCE

List partition allows us to segment data based on a pre-defined set of values (e.g. 1, 2, 3), this is done by using PARTITION BY LIST(expr) where expr is a column value and then defining each partition by means of a VALUES IN (value_list), where value_list is a comma-separated list of integers.

OUTPUT:

![image](https://user-images.githubusercontent.com/73202856/103324480-a8047380-4a82-11eb-965b-469fe6134802.png)

# Query 2 – SQL Partitions  ( LIST COLUMNS Partitioning )

CREATE TABLE salemast (
     agent_id VARCHAR(15),
     agent_name VARCHAR(50), 
     agent_address VARCHAR(100),
     city_code VARCHAR(10)
) 
PARTITION BY LIST COLUMNS(agent_id) ( 
     PARTITION pcity_a VALUES IN('A1', 'A2', 'A3'), 
     PARTITION pcity_b VALUES IN('B1', 'B2', 'B3'), 
     PARTITION pcity_c VALUES IN ('C1', 'C2', 'C3', 'C4', 'C5')
)
;

- in a company there are agents in 3 cities, for sales and marketing purposes, with this we organized those agents in these 3 cities.

# Importance
LIST COLUMNS accepts a list of one or more columns as partition keys, you can also use various columns of data of types other than integer types as partitioning columns.

# Output:
![image](https://user-images.githubusercontent.com/73202856/103324564-03cefc80-4a83-11eb-97e4-5d9367d8d9eb.png)

# Query 3 - SQL Partitions ( Subpartitioning )

CREATE TABLE table10 (
     BILL_NO INT,
     sale_date DATE,
     cust_code VARCHAR(15), 
     AMOUNT DECIMAL(8,2)
)
PARTITION BY RANGE(YEAR (sale_date))
SUBPARTITION BY HASH(TO_DAYS (sale_date))
SUBPARTITIONS 4 (
     PARTITION p0 VALUES LESS THAN (1990),
     PARTITION p1 VALUES LESS THAN (2000),
     PARTITION p2 VALUES LESS THAN (2010),
     PARTITION p3 VALUES LESS THAN MAXVALUE
)
;

- table has 4 RANGE partitions and each of these partitions; p0, p1, p2 and p3 is further divided into 4 subpartitions, therefore the table is divided into 4 x 4 = 16 partitions.

# Importance
Subpartitioning is a method to divide each partition further in a partitioned table.

# Output:
![image](https://user-images.githubusercontent.com/73202856/103324614-3bd63f80-4a83-11eb-9362-8d06e4267e24.png)

# Query 4 - SQL Ranking Function

SELECT subjects, s_name, mark, dense_rank()
OVER ( partition by subjects order by mark desc )
AS ‘dense_mark’ FROM result;

- dense_rank(). Table is partitioned on the basis of “subjects”. order by clause is used to arrange rows of each partition in descending order by “mark”. dense_rank() is used to rank students in each subject.

# Importance
This function will assign rank to each row within a partition without gaps. The ranks are assigned in a consecutive manner, if there is a tie between values then they will be assigned the same rank, and next rank value will be one greater then the previous rank assigned.

# Output:

Before

![image](https://user-images.githubusercontent.com/73202856/103324672-7fc94480-4a83-11eb-8bd9-fb42163719ee.png)

After

![image](https://user-images.githubusercontent.com/73202856/103324679-8788e900-4a83-11eb-91f1-4cf21b5bc157.png)

# Query 5 - SQL Ranking Function

SELECT subjects, s_name, mark, rank()
OVER ( partition by subjects order by mark desc )
AS ‘rank’ FROM result; 

- rank(). It’s output is similar to dense_rank() function. Except, that for Science subject in case of a tie between Ankita and Pratibha, the next rank value is incremented by 2 i.e 3 for Swarna.

# Importance
This function will assign rank to each row within a partition with gaps. Ranks are assigned in a non-consecutive manner, if there is a tie between values then they will be assigned same rank, and next rank value will be previous rank + no of peers(duplicates). 

# Output:

Before
![image](https://user-images.githubusercontent.com/73202856/103324719-a5564e00-4a83-11eb-8aca-cd41f1852d84.png)

After
![image](https://user-images.githubusercontent.com/73202856/103324728-b1daa680-4a83-11eb-81b9-a9ef712338ed.png)

# Query 6 – SQL Ranking Function
SELECT subjects, s_name, mark, percent_rank()
OVER ( partition by subjects order by mark )
AS ‘percent_rank’ FROM result;
- percent_rank(). The percent_rank() function calculate percentile rank in ascending order by “mark” column. rank is the rank of each row of the partition resulted using rank() function. rows represent the no of rows in that partition.

# Importance
It returns the percentile rank of a row within a partition that ranges from 0 to 1. It tells the percentage of partition values less than the value in the current row, excluding the highest value.

# Output:

Before
![image](https://user-images.githubusercontent.com/73202856/103324812-e8b0bc80-4a83-11eb-9f5c-c35003b8fead.png)

After
![image](https://user-images.githubusercontent.com/73202856/103324840-f49c7e80-4a83-11eb-99bd-8f24276c2fd9.png)

# Query 7 – SQL Trigger

CREATE TRIGGER `insertLog` AFTER INSERT ON `authors` FOR EACH ROW INSERT INTO logs VALUES(null, NEW.id, 'Inserted', NOW());

- creates a Trigger in the designated table that registers the inserted data into the logs whenever an insert event had happened.

# Importance
Automatically registers any insert event happened on the table where the Trigger is created into the designated table, making manual input not necessary and insert event registration relatively fast.

#Output:

Insert data:
![image](https://user-images.githubusercontent.com/73202856/103324871-1564d400-4a84-11eb-9a41-bb82443ca015.png)

Then see if the data matches with the logs table:
![image](https://user-images.githubusercontent.com/73202856/103324879-21509600-4a84-11eb-8b80-1b0ab6b62609.png)

# Query 8 – SQL Trigger

CREATE TRIGGER `updateLog` AFTER UPDATE ON `authors` FOR EACH ROW INSERT INTO logs VALUES(null, NEW.id, 'updated', NOW());

- creates a Trigger in the designated table that registers the updated data into the logs whenever an update event had happened.

# Importance
Automatically records any update event happened on the table where the Trigger is created into the designated table making update event happened known in detail.

# Output:

Update Data:
![image](https://user-images.githubusercontent.com/73202856/103324901-388f8380-4a84-11eb-9211-4880f1689888.png)

See logs table if update event is correct:
![image](https://user-images.githubusercontent.com/73202856/103324917-42b18200-4a84-11eb-9eb9-64fcb6d872f0.png)

# Query 9 – SQL Trigger

CREATE TRIGGER 'deleteLog' BEFORE INSERT ON 'authors' FOR EACH ROW INSERT INTO logs VALUES(null, OLD.id, 'Deleted', NOW());

- creates a Trigger in the designated table that registers the deleted data into the logs whenever a delete event had happened.

# Importance
Automatically records delete data on the table the Trigger is created into the designated table thus making any delete event recorded.

# Output:

Delete any data in the table where the Trigger is created then see if its recorded in the appointed table:
![image](https://user-images.githubusercontent.com/73202856/103324941-5e1c8d00-4a84-11eb-8408-bcd1f439d313.pn

# Query 10 – SQL Transactions

DELETE FROM Student WHERE AGE = 20;
COMMIT;

- this query deletes the records from the table which have age = 20 and then COMMIT the changes in the database.

# Importance
If everything is in order with all statements within a single transaction, all changes are recorded together in the database is called committed.

# Output:

Before
![image](https://user-images.githubusercontent.com/73202856/103324968-7db3b580-4a84-11eb-8eac-ff6761b36047.png)

After
![image](https://user-images.githubusercontent.com/73202856/103324975-8906e100-4a84-11eb-8d84-7553c66a5396.png)

# Query 11 – Dynamic Query

DECLARE 
@tab NVARCHAR(128), 
@st NVARCHAR(MAX);
SET @tab = N'geektable';
SET @st = N'SELECT * 
FROM ' + @tab;
EXEC sp_executesql @st;

- selects the data from a complex table ( these are SQL Server Syntax ).

# Importance
Using Dynamic SQL is less swift and efficient but is flexible, often used in old and very complex database.

# Output:
![image](https://user-images.githubusercontent.com/73202856/103325172-8789e880-4a85-11eb-9706-ab0e4ad2f39d.png)

# Query 12 – SQL Views

DROP VIEW Skirts;

- deletes the view you created.

# Importance
When mistakes are made you can easily drop a VIEW.

# Output:
![image](https://user-images.githubusercontent.com/73202856/103325189-9e303f80-4a85-11eb-8c20-5c9d8a824714.png)

# Query 13 – SQL Views ( Updating Views )

CREATE OR REPLACE VIEW Skirts AS
SELECT StockID, name, topline
FROM stock
WHERE categoryID = '1';

- replaces the created VIEW with another VIEW on the above command.

# Importance
Updating or replacing a VIEW is important, specially when adding or deleting viewing data.

# Output:
![image](https://user-images.githubusercontent.com/73202856/103325216-c029c200-4a85-11eb-9e95-16abb86cf8e6.png)

# Query 14 – SQL Views

CREATE VIEW Skirts AS
SELECT StockID, name
FROM stock
WHERE categoryID = '1';

- creates a virtual table to view all stock that has categoryID that is equal to 1.

# Importance
Creates a viewing table that cointains data without having to create a real table.

# Output:
![image](https://user-images.githubusercontent.com/73202856/103325238-ddf72700-4a85-11eb-9048-931cfcedccfe.png)

# Query 15 – Store Procedure

SP_DEPENDS SelectGeek ;

- will show where the procedure is dependent like name of tables, functions, etc.

# Importance
Depencies will be quickly identified, making dependency diagrams relatively easy.

# Output:
![image](https://user-images.githubusercontent.com/73202856/103325261-f6ffd800-4a85-11eb-8393-2d8636eec536.png)

# Query 16 – Stored Procedure

SP_HELPTEXT SelectGeek ;

- will display the content of the stored procedure as result.

# Importance
You can easily generate back the queries used in the creation of the Stored Procedure.

# Output:
![image](https://user-images.githubusercontent.com/73202856/103325293-1eef3b80-4a86-11eb-9a99-81eb23644ec6.png)

# Query 17 – Stored Procedure

SP_HELP SelectGeek ;

- will display the Stored procedure Name, Schema Name, created date, and Time or if there are any parameters, then Parameter Name, Data Type, Length, Precision, Scale, Collation, etc. as result.

# Importance
Specific detail of the creation of  a store procedure can be seen.

# Output:
![image](https://user-images.githubusercontent.com/73202856/103325318-3f1efa80-4a86-11eb-810d-0dc496e75664.png)

# Query 18 – Stored Procedure

CREATE PROCEDURE SelectGeek
AS
BEGIN
SELECT TOP 3 [Name], [City], [Salary]
FROM [geek_demo]
ORDER BY [Salary] ASC
SELECT TOP 3 [Name], [City], [Salary]
FROM [geek_demo]
ORDER BY [Salary] DESC
END
GO;

//Call the stored procedure

EXEC SelectGeek ;

- creates a simple stored procure that holds two Select statements inside it from the selected table.

# Importance
Creates a stored procedure that you can save, so the code can be reused over and over again.

# Output:

Selected Table
![image](https://user-images.githubusercontent.com/73202856/103325341-6675c780-4a86-11eb-967d-1e26bc9a1ae3.png)

Stored Procedure
![image](https://user-images.githubusercontent.com/73202856/103325350-6e356c00-4a86-11eb-80de-0aada52c46e5.png)

# Query 19 – SQL Transactions

SAVEPOINT SP1;

//Savepoint created.

DELETE FROM Student WHERE AGE = 20;

//deleted

SAVEPOINT SP2;

//Savepoint created.

-  SP1 is the first SAVEPOINT created before deletion then after deletion, SAVEPOINT SP2 is created.

# Importance
A SAVEPOINT is a point in a transaction in which you can roll the transaction back to a certain point without rolling back the entire transaction. 

# Output:
![image](https://user-images.githubusercontent.com/73202856/103325380-8f965800-4a86-11eb-9522-3a58f3d042ae.png)

# Query 20 – SQL Transactions

DELETE FROM Student WHERE AGE = 20;
ROLLBACK;

- deleted records will be specifically retrieved back in the database.

# Importance
If any error occurs with any of the SQL grouped statements, all changes need to be aborted. The process of reversing changes is called rollback.

# Output:
![image](https://user-images.githubusercontent.com/73202856/103325393-a50b8200-4a86-11eb-8867-260e52853333.png)
