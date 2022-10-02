<h1 style="text-align: center;"> MySQL Reference </h1>

## Getting Started:

- **What's a Database:**    
    1. A Collection of Data
	2. Has methods for manipulations

- **Database Management System:**
    - Acronyms: DBMS, RDMS
	- Code talks to DBMS, DMS talks to Database
	- DMSs: MySQL, PostgreSQL, etc...

- **MySQL vs SQL:**		
    - SQL is the language to "talk" to our database
	- MySQL is a database management system: It uses SQL the language, like many other DBMSs do.
    - What makes DBMSs different, are the features they provide: like security, download speed, etc...
	- goormIDE:			
        - used so we don't have to install MySQL on our local machine
		- to start server: 	`mysql-ctl start`
		- to stop server:	`mysql-ctl stop`
		- MySQL shell:		`mysql-ctl cli`
		- Exit MySQL shell:	`exit` or `^C`
		- CLI Commands:
			- `create database <name>;`
			- `use <name>;`
			- `source <query.sql>` (to run SQL code by filename);
			- `desc <tablename>;`

## Creating Databases and Tables:

- Creating Databases
    - You can have multiple databases inside one database server
    - We need to create individual databases to wall off communication among them
    - People capitalize sql stuff and lowercase variables/expressions (for convention)

    |||
    |---|---|
    | Show Databases: |		`SHOW DATABASES;`|
    | Create Databases: |		`CREATE DATABASE <name>;` |
	| Delete Database: |		`DROP DATABASE <name>;` |
    | Tell  MySQL what database we want to work with: | `USE <database-name>;`|
    | Find out the currently used database: | `SELECT database();` (NULL means not currently using any) |

- Intro to Tables: ("the heart of SQL")		
    - A database is just a bunch of tables (in relational databases, at least)
	- Tables hold the data ("a collection of related data held in a structured format within a DB")
	- Terminology:
	    - **Columns** (headers) - &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Name | Breed | Age
	    - **Rows** (the actual data) - &nbsp;blue | Yorkie| 2

- The Basic Datatypes:
    - Dataypes are important to avoid inconsistent data Ex:(Age: 1, 3, ten)
    - MySQL enforces Datatypes when making tables

    |Numeric Types	|		String Type	|		Date Types |
    | :---: | :---: | :---: |
    | INT			| CHAR			| DATE  |
    | SMALLINT		| VARCHAR		| DATETIME |
    | TINYINT		| BINARY		| TIMESTAMP |
    | MEDIUMINT		| VARBINARY		| TIME |
    | BIGINT		| BLOB			| YEAR |
    | DECIMAL		| TINYBLOB      ||
    | NUMERIC		| MEDIUMBLOB    ||
    | FLOAT			| LONGBLOB      ||
    | DOUBLE		| TEXT          ||
    | BIT			| TINYTEXT      ||
    |               | MEDIUMTEXT    ||
    |               | LONGTEXT      ||
    |               | ENUM          ||
    
    - We'll work with **INT**(whole number) for numbers and **VARCHAR**(variable-length) for strings
        where **CHAR** is a fixed length where everything in that column has to be the same length
        but **VARCHAR** let's us have variation between 1 and 255 characters
        
- **How to actually create tables:**
    ```sql
    CREATE TABLE tablename
    (
        column_name data_type,
        column_name data_type
    );
    ```
    - Example -
        ```sql
        CREATE TABLE cats
        (
            name VARCHAR(100),
            age INT
        );
        ```

|||
|---|---|
| Show Tables in a DB | 	`SHOW TABLES;` |
| Show Columns of a Table |	`SHOW COLUMNS FROM <tablename>; or DESC <tablename>;` |
| Deleting Tables |		`$ DROP TABLE <tablename>;`|

## Inserting data (and others):
- **Inserting Data -**
    ```sql
    INSERT INTO someTable( someColName1, someColName2, ...)
    VALUES (valForSomeColName1, valForSomeColName2, ...),
            (valForSomeColName1, valForSomeColName2, ...),
            (valForSomeColName1, valForSomeColName2, ...);
    ```
- To View all of our data in a given table - 
    ```sql
    SELECT * FROM someTable;
    ```	

- MySQL Warnings:
    ```sql
    SHOW WARNINGS;
    ```
    - We saw errors where long strings were cut off. And a string value fed to an INT was replaced by a default 0.

- NULL and NOT_NULL:		
    - NULL is a default for a column when you CREATE a table
        - YES value on a NULL means that it permits the column value to be NULL
        - To change values to NOT NULL during creation we do:
            ```sql
            CREATE TABLE <table-name> (
                colName valType NOT NULL,
                colName valType NOT NULL,
                ...
            )
            ```
            - IF you try to insert a NULL into a cell that can't be NULL, it uses its default value.

- Setting Default Values:	
    - A default value is a fallback if we get warnings.
    - To set default values during creation we do:
        ```sql
        CREATE TABLE someTable(
            colName valType DEFAULT 'someValidType',
            colName valType DEFAULT someValidType,
            ...
        )
        ```
    - You can put NOT NULL before the DEFAULT to mix.

- A Primer on Primary Keys:	
    - A PRIMARY KEY IS A UNIQUE IDENTIFIER
    - To specify a primary key during creation:
        ```sql
        CREATE TABLE someTable(
            cat_id INT NOT NULL AUTO INCREMENT,
            colName valType NOT NULL,
            colName valType NOT NULL,
            PRIMARY KEY (cat_id)	//Says "by the way that cat_id I mentioned earlier, is the primary key"
        )
        ```
        - **AUTO INCREMENT** makes It so when we insert. We don't have to specify the cat_id with a value.
            SQL will jus keep incrementing it after every insertion, which ensures it'll be unique.
        - **PRIMARY KEY** can also be placed beside **AUTO INCREMENT**.

## CRUD commands (Create Read Update Delete)
- READ(SELECT):
	``` sql
	SELECT * FROM cats; 			--The (*) means "Give me all columns"
	SELECT colName FROM cats; 		--To show a specific column
	SELECT col1, col2 FROM cats;	--To show more than 1 column
	```
- WHERE (to get speciific):	
	```sql
	SELECT * FROM cats WHERE age=4	--for strings quality is case insensitive	
									--can be something like colName = colName to compare
	```
- ALIASES:			
	```sql
	SELECT cat_id AS id FROM cats	--Gives the column name an alias (useful for joining tables)
	```
- UPDATE(alter existing data):
	```sql
	UPDATE cats SET breed='Shorthair'
	WHERE breed='Tabby';			--IT'S A GOOD IDEA TO TEST A WHERE BEFORE YOU ACTUALLY UPDATE!NO UNDOS!!!!!
	```
- DELETE:	
	```sql
	DELETE FROM cats WHERE name='Egg' 	--Very similar to select
	DELETE FROM <table-name>			--Deletes everything from the table not the table itself
	```				

## The World of String Functions
- **CONCAT()** (Combine Data For Cleaner Output)
	```sql
	CONCAT( col, anotherCol, etcCol...)
	```	
	- Situation
		- You separate a full name into first & last names, but then you want to CONCAT it to a full name.
	- Notes		
		- Has to be called within a SELECT	(Ex: `SELECT CONCAT('Hello', ' ', 'World')` )
		- Use FROM to reference tables		(Ex: `SELECT CONCAT(col1, col2) FROM someTable;`)
		- `CONCAT_WS (separator, col1, col2, ...)`	adds a separator between each concatanation.

- **SUBSTRING()** (Work w/ parts of strings)
	```sql
	SELECT SUBSTRING(someString, 1, 4);	--MYSQL INDICES START AT ONE!!!!!!!!!!!!!
	```		
	- Notes
		- If you only specify one index then it goes from that index to the end of the string.
		- Negative numbers are possible. They start from the end of the string.
	- Important		
		- Used with tables maybe like so: `SELECT SUBSTRING(colName, 1, 10) AS 'short title' FROM tableName;`
		- You can mix SUBSTRING w/ CONCAT

- REPLACE() (replace parts of a string)	
	```sql
	SELECT REPLACE('Hello World', 'Hell', '****')
	```
	- Syntax Notes
		- *Argument 1:* string in question	 
		- *Argument 2:* what we ant to replace 
		- *Argument 3:* the thing we'll use to replace
	- Important		
		- It's case sensitive (ex: o doesn't replace O)
		- Again, for tables you can do FROM
		- Also mixable w/ other string functions

- REVERSE() (reverses a string)
	```sql
	SELECT REVERSE('HELLO WORLD')	--FROM to reference table
	```

- CHAR_LENGTH() (counts chars in string)
	```sql
	SELECT CHAR_LENGTH('HELLO WORLD') --FROM to reference table
	```

- UPPER() & LOWER() (change a strings case)
	```sql
	SELECT UPPER('Hello World')	--FROM; LOWER is used used the exact way
	```


## Refining Our Selections
- Using DISTINCT		
	```sql
	SELECT DISTINCT some_col_name FROM some_table;
	```
	- Important		
		- It gets non-dupicate entries
		- When you specify more than one column. It runs distinct on those columns as a row (every entry must be distinct to another)

- Sorting Data w/ ORDER BY
	```sql
	SELECT some_col_name FROM some_table ORDER BY some_col_name;
	SELECT some_col_name_1, some_col_name_2, some_col_name_3 FROM some_table ORDER BY 2; 
		-- Where 2 refers to some_col_name_2 the order of what we selected
	```		
	- Important		
		- For alphabetical (ascending) - just specify the some-col-name
		- For alphabetical (descending) - add DESC to ascending
		- Also works for Numbers
		- You can ORDER BY more than 1 column. Ex: `ORDER BY lname, fname;` This means after we do our initial sorting we can sort by fname if there are any conflicts such as people having the same last name so we have to sort their first names

- Using LIMIT:
	```sql
	SELECT some_col_name FROM some_table LIMIT 10;
	SELECT title, released_year FROM books ORDER BY released_year DESC LIMIT 5 --Described in "Situation"
	```	
	- Situation		
		- Specify a number of entries you want to limit a query to 
		- Usually used with `ORDER BY` to do a query such as "5 most recently released books"
	- Important		
		- You can have the notation `LIMIT 5, 10` where the first number (5) means we start at the 5th row and the second number ,10, is the actual limit after the 5th row. **THIS MAY BE GOOD FOR PAGINATION**

- Better Searches with LIKE
	```sql
	WHERE author_fname LIKE '%da%' -- here the % are wildcards. Anything comes before and after
	WHERE stock_quantity LIKE '____'	
	-- 4 UNDERSCORES. So a stock_quantity where there are FOUR CHARACTERS (any four)
	-- So it may be a digit 1000 and this query would get it, but nothing that has more or less digits
	```
	- Situation		
		- "*There's this book I'm looking for. But I can't rememeber the title! I think the author's name is Dave or Dan or David...*" (This is similar to WHERE except WHERE wants exact matches)
	- Important		
		- To actually use % or _ we can use the escape character \ before them


## The Magic of Aggregate Functions
- The Count Function:
	```sql
	SELECT COUNT(*) FROM someTable;
	SELECT COUNT(DISTINCT author_fname, author_lname) FROM books;	--To only count unique values
	SELECT COUNT(*) FROM books WHERE title LIKE '%the%';
	```
	- Situation		
		- "How many books are in the database?

- The Joys of Group By:
	```sql
	SELECT author_lname, COUNT(*) FROM books GROUP BY author_lname;  --Yields a table with COUNTS for each GROUP
	SELECT CONCAT('In ', released_year, COUNT(*), ' book(s) released' FROM books GROUP BY released_year; --"How many books released each year?"
	```
	- Explanation:		
		- Harder to use because you have to use it with other functions.
		- It's how we can do things like finding the min, max, average, etc.
		- "`GROUP BY` summarizes or aggregates identical data into single rows"
	- Situation:		
		- "*Group movies by genre and tell me how many movies each genre has*"

- Min and Max Basics
	```sql
	SELECT MIN(released_year) FROM books; --Replace MIN w/ MAX to do the same thing
	```
	- Situation
		- Identify Min/Max value in a table (can be used on its own OR combined with GROUP BY)
	- WARNING!!!!!		
		- You can't do "SELECT MIN(released_year), title FROM books" and expect title to be the title that corresponds to the minimum value
		- The solution is.... SUBQUERIES....
- SUBQUERIES - A problem with Min and Max
	```sql
	SELECT * FROM books
	WHERE pages = (SELECT Min(pages) FROM books);
	```			
	- Situation		
		- ABOVE 
	- Explanation		
		- Where the SUBQUERY is the SQL statement in parenthesis
	- Alternative Method		
		```sql
		SELECT * FROM books
		ORDER BY pages ASC LIMIT 1;		-- or DESC
		```

- Using Min and Max w/ Group By	
	```sql
	SELECT 	author_fname,
			author_lname,
			Min(released_year)
	FROM	books
	GROUP BY 	author_lname,
				author_fname;

			--OR
	SELECT
		CONCAT(author_fname, ' ', author_lname) AS author,
		MAX(pages) AS 'longest book'
	FROM books
	GROUP BY 	author_lname,
				author_fname;
	```
	- Scenarios		
		- "Find the year each author published their first book"			

- The Sum Function
	```sql
	SELECT Sum(pages) FROM books;
	SELECT author_fname, author_lname, Sum(pages)
		FROM books
		GROUP BY author_lname, author_fname;
	```
	- Scenario		
		- "*Sum all pages in the entire database*"
		- "*Sum all pages each author has written*"

- The Avg Function
	```sql
	SELECT AVG(released_year) FROM books;
	SELECT AVG(stock) FROM books GROUP BY released_year;
	```
	- Scenarios		
		- "Calculate the average released_year across all books
		- "Calculate the average stock quantity for books released in the same year"

## The Power of Logical Operators
- Not Equal
	```sql
	SELECT title FROM books WHERE year != 2017;
	```			
	- Scenario		
		- "*Select all books NOT published in 2017*"

- Not Like
	```sql
	SELECT title FROM books WHERE title NOT LIKE 'W%';
	```
	- Scenario		
		- "*Select all books that do not start with a W*"

- Greater Than
	```sql
	SELECT * FROM books WHERE released_year > 2000 ORDER BY released_year;
	```			
	- Scenario		
		- "*Select all books that were released after the year 2000*"
	- Notes			
		- There is `>=` "Greater than or Equal to"
		- `SELECT 99 > 1` yields 1 for TRUE (0 would be false)

- Less Than
	- Works the same as Greater Than but with `<`

- Logical AND (&&)
	```sql
	SELECT * FROM books WHERE author_fname="Eggers";
	SELECT * FROM books WHERE released_year > 2010;
		-- TURNS INTO
	SELECT * FROM books WHERE author_lname="Eggers" AND released_year > 2010;
	```
	- Scenario		
		- "*SELECT books written by Dave Eggers, published after the year 2010*"
	- Notes
		- You can do more than 1 AND.
		- You can use AND or &&

- Logical OR (||)		
	- Similar to AND

- Between			
	- Scenarios		
		- Select thing based off of an upper and lower range
		- "Select all books published BETWEEN 2004 and 2015"
	- Without `BETWEEN` we can do the scenario with `AND` as follows -			
		```sql
		SELECT title, released_year 
		FROM books 
		WHERE released_year >= 2004 && released_year <= 2015;
		```
	- Using `BETWEEN`
		```sql		
		SELECT title, released_year FROM book
		WHERE released_year BETWEEN 2004 AND 2015;
		```
	- Notes:
		- You can use `NOT BETWEEN` to get things that aren't in the specified range			
		- Use `CAST()` when trying to use `BETWEEN` with `DATE` values. Convert them to `DATETIME`
			- Ex:	`SELECT CAST('2017-05-02' AS DATETIME);`
		
- IN and NOT IN
	```sql
	SELECT title, author_lname FROM books
	WHERE author_lname IN ('Carver'. 'Lahiri', 'Smith');

	SELECT title, author_lname FROM books
	WHERE 	released_year >= 2000 AND
			author_lname NOT IN ('Carver'. 'Lahiri', 'Smith');
	```
	- Scenario		
		- "Select all books written by Carver or Lahiri or Smith"
		- We may do many `OR` queries but `IN` provides a sleeker query
	- Notes			
		- `NOT IN` does the opposite of `IN`
		- Use Modulus to check for even-ness
			```sql
			SELECT title, released_year FROM books
			WHERE released_year >= 2000 AND
			released_year % 2 != 0;
			```
								
- Case Statements
	```sql
	SELECT title, released_year
	CASE
		WHEN released_year >= 2000 THEN 'Modern Lit'
		ELSE '20th Century Lit'
	END AS GENRE
	FROM books;

	SELECT title, stock_quantity,
	CASE
		WHEN stock_quantity BETWEEN 0 AND 50 THEN '*'
		WHEN stock_quantity BETWEEN 51 AND 100 THEN '**'
		ELSE '***'
	END AS stock
	FROM books;
	```
	- Scenario		
		- Conditional Statements
	- IMPORTANT		
		- Above: We use `AS GENRE` because otherwise the HEADER would be the entire CASE to END, therefore it's neater to name it something like GENRE
		- Open and close CASES with CASE/END pairs
	- Notes			
		- Can be more succient with <= and realizing the order of execution

One To Many:
	-Real World Data		-Notes:			* So far we've been working with one table (simple data)
	 is Messy:						* Real world data is Messy and Interrelated

	-Types of Data			-Relationships:		1) One to One Relationship - (Not common) - A Users detail belongs to one User, and a User only uses those details
	 Relationships:						2) One to Many Relationship (Most Common) - Books can have many Reviews, but a Review Belongs to One book
								3) Many to Many Relationship - "Books can have many authors, and those authors can have many books"

	-One to Many			-Scenario:		* Customers and Orders: One customer can have multiple orders
	 The Basics:							** We may want to store a customer's first & last name, email, purchase date, order price.
								* We could store this info with:
									1) One Table:
										-but it has the drawback of repeatedly storing orders with the same customer
										 so we do...
									2) One Table to Many Tables:
										-so orders would have an id (seperate from their primary key) that links them to ONE customer
										-For One Table if a customer hadn't placed an order. We would have had to store NULLs, but this way we simply don't store
										 any order that links to the customer
					-IMPORTANT:		* The id used aside from PRIMARY KEYs, are the references to other tables within a given table
								  which are called FOREIGN KEYs

	-Working with			-Syntax:		$ CREATE TABLE orders(
	 Foreign Keys:							// Column specifications here
									customer_id INT,										//Naming convention for Foreign Key name for this table:	The name of the Table and the name of the Primary Keys column
									FOREIGN KEY(customer_id) REFERENCES nameOfSomeOtherTable(nameOfSomeOtherTablesPrimaryKey)
								);

					-Notes:			* When we define a FOREIGN KEY, if we try to INSERT something that isn't a proper REFERENCE then MySQL gives an error

	-Cross Join:			-Scenario:		* "Find all orders placed by Boy George"
								  We can do this in three ways:
									1) Look up Boy George's ID with one query, and then Look up all entries that match that id in orders with another query.
									2) Do (1) in query using subqueries
									* Though none of these are ideal because we can't really join the headers of one table with another. This is where JOINs come in.
									3) JOINS (a basic type of join is called a "Cross Join". It's rarely used because it yields duplicated data amongst two tables.)

					-Syntax:		$ SELECT * FROM orders WHERE customer_id = 
									(SELECT id FROM customers WHERE last_name="George")		//This is a subquery

								* Cross Join (rarely used):
									$ SELECT * FROM customers, orders;

	-Inner Join:			-Scenario:		* In order to widdle down the useless Cross Join which yields multiples we have to filter using WHERE
								* There are two ways to the same thing: IMPLICIT and EXPLICIT

					-Syntax:		* IMPLICIT INNER JOIN:	
									$ SELECT * FROM customers, orders
								  	WHERE customers.id = orders.customer_id;

								* EXPLICIT INNER JOIN (used by most devs):
									$SELECT * FROM customers
									 JOIN orders
										ON customers.id = orders.customer_id;

					-Problems:		* In order to safeguard from similarly named columns in two tables, we can use the dot (.) syntax prepended by each
								  table's name

					-Visualization:		* Think of Venn Diagrams:
									** A Cross Join yeilds multiples because it's two circle overlapping
									** An Inner Join just gets the inner circle where they overlap
								
					-Notes:			* Order of how you comma seperate the tables matters in the sense of how your data is presented to you
								* You can treat these JOINs as regular graphs that you can GROUP BY, use AS, etc...
								* JOIN can be INNER JOIN

	-Left Join:			-Scenario:		* There are other types of join besider 'inner join'. We have left and right joins (here we cover left)
								* Thinking in terms of the Venn Diagram we'll get the Left Circle from the Left join
								* "Select everything from A, along with any matching records in B"
								* "For every user how much have they spent? Even if they haven't ordered anything"

					-Syntax:		$SELECT * FROM customers
								 LEFT JOIN orders
									ON customers.id = orders.customer_id;

					-Notes:			* So it includes everything from table A along with corresponding things for table B, but for anything else in B it comes in as NULL by default 
								  which can be modified by using:
									$SELECT first_name, last_name, IFNULL(SUM(amount), 0) FROM customers LEFT JOIN orders //etc...

	-Right Join:			-Scenario:		* "Right side of Venn Diagram"

					-Syntax:		$SELECT * FROM customers 
								 RIGHT JOIN orders
									ON customers.id = orders.customer_id;
					-IMPORTANT:		*Works the same as left join. THE IMPORTANT THING HERE IS WE NOTE THINGS WHEN WE ATTEMPT TO BREAK OUR DATA.
									** When we try to delete data that FOREIGN KEYS are DEPENDENT to, it raises an error. (Even DROP table doesn't work)
									   FIX: *** DROP tables in order

	-How to DELETE			-Syntax:		* YOU MUST DO THIS WHEN DEFINING YOUR FOREIGN KEY:
	 when tied down							$ FOREIGN KEY(customer_id) REFERENCES customers(id) ON DELETE CASCADE;
	 by a FOREIGN KEY:						"When a customer is deleted that has a corresponding order, delete the order as well"
								* NOW REGULAR DELETE SHOULD WORK

Many To Many:
	-Many to Many Basics:		-IMPORTANT:		* In order to make a Many to Many Relationship, WE HAVE TO MAKE A NEW TABLE that has FOREIGN KEY references to the tables that are apart of this Many to Many 
								  relationship
					-Scenario:		* "Series can have many Reviewers. Reviewers can have many Series reviewd. We make a NEW TABLE called REVIEWS that has ID pointers to  the Reviewers Table and
								  Series Table"

	-Creating Our Tables:		-Syntax:		*Creating the Reviewers Table:
									CREATE TABLE reviewers (
        									id INT AUTO_INCREMENT PRIMARY KEY,
        									first_name VARCHAR(100),
        									last_name VARCHAR(100)
    									);

								*Creating the Series Table:
    									CREATE TABLE series(
       										id INT AUTO_INCREMENT PRIMARY KEY,
        									title VARCHAR(100),
        									released_year YEAR(4),
        									genre VARCHAR(100)
    									);

								*Creating the Reviews Table:
    									CREATE TABLE reviews (
        									id INT AUTO_INCREMENT PRIMARY KEY,
        									rating DECIMAL(2,1),
        									series_id INT,
        									reviewer_id INT,
        									FOREIGN KEY(series_id) REFERENCES series(id),
        									FOREIGN KEY(reviewer_id) REFERENCES reviewers(id)
    									);
								*Then we can do Insertions

	-Challenges:			-Problems:		1) Inner Join
								2) Average
								3) Simple Inner Join for more info
								4) Finding Non reviews with LEFT JOIN:
									* SELECT * FROM someTable LEFT JOIN reviews ON something WHERE rating IS NULL;
								5) Average Reviews by Genre
									*ROUND(toRound, 2) toRound by 2 decimal places
								6) Put it all together
								7) Put together all three tables
									*You can chain together multiple JOINs

INSTAGRMA DATABASE CLONE
	-Users Schema:			-Syntax:			$ CREATE TABLE users (
        									id INTEGER AUTO_INCREMENT PRIMARY KEY,
        									username VARCHAR(255) UNIQUE NOT NULL,
        									created_at TIMESTAMP DEFAULT NOW()
    								  );

	-Photos Schema:			-Syntax:			$ CREATE TABLE photos (
        									id INTEGER AUTO_INCREMENT PRIMARY KEY,
        									image_url VARCHAR(255) NOT NULL,
        									user_id INTEGER NOT NULL,
        									created_at TIMESTAMP DEFAULT NOW(),
        									FOREIGN KEY(user_id) REFERENCES users(id)
    								  );

	-Comments Schema:		-Syntax:			$ CREATE TABLE comments (
        									id INTEGER AUTO_INCREMENT PRIMARY KEY,
        									comment_text VARCHAR(255) NOT NULL,
        									photo_id INTEGER NOT NULL,
        									user_id INTEGER NOT NULL,
        									created_at TIMESTAMP DEFAULT NOW(),
        									FOREIGN KEY(photo_id) REFERENCES photos(id),
        									FOREIGN KEY(user_id) REFERENCES users(id)
    								  );

	-Likes Schema:			-Syntax:			$ CREATE TABLE likes (
        									user_id INTEGER NOT NULL,
        									photo_id INTEGER NOT NULL,
        									created_at TIMESTAMP DEFAULT NOW(),
        									FOREIGN KEY(user_id) REFERENCES users(id),
        									FOREIGN KEY(photo_id) REFERENCES photos(id),
        									PRIMARY KEY(user_id, photo_id).
    								  );

					-IMPORTANT:		* NOTE: We store a pair in a PRIMARY KEY. It has the effect if "only one combination of those two parameters
								        can exist. Therefore, in this case, we can't spam the Like button.

	-Followers Schema:		-Syntax:			$ CREATE TABLE follows (
        									follower_id INTEGER NOT NULL,
        									followee_id INTEGER NOT NULL,
        									created_at TIMESTAMP DEFAULT NOW(),
        									FOREIGN KEY(follower_id) REFERENCES users(id),
        									FOREIGN KEY(followee_id) REFERENCES users(id),
        									PRIMARY KEY(follower_id, followee_id)			//Like Likes Schema so we can't spam follow someone
    								  );

	-Hashtag Schema:			-Three popular		1) A new column that stores entries that contain strings in the form of "tag1#tag2#tag3#etc..." 
					 solutions:			* Easy to implement, but is limited to the string cap
	
								2) A Tags TABLE that contains REFERENCES to photos where it is used
									* Unlimited number of Tags available, but slower.

								3) Two TABLEs one that contains the tags name and id and another that looks up that tag_id and links it to a photo_id
									* No repeated tag string +
									* Harder to work with because of dependencies

					-We choose (3)		
					 so its Syntax:		$ CREATE TABLE tags (
      									id INTEGER AUTO_INCREMENT PRIMARY KEY,
      									tag_name VARCHAR(255) UNIQUE,
      									created_at TIMESTAMP DEFAULT NOW()
    								  );
    								  CREATE TABLE photo_tags (
        									photo_id INTEGER NOT NULL,
        									tag_id INTEGER NOT NULL,
        									FOREIGN KEY(photo_id) REFERENCES photos(id),
        									FOREIGN KEY(tag_id) REFERENCES tags(id),
        									PRIMARY KEY(photo_id, tag_id)
    								  );

INTRODUCING NODE:
	-Connecting Node to		Three Steps:		1) We need an adapter that our language of choice uses to speak to the MySQL server.
	 MySQL:							   in the case of Node we use NPM to install the MySQL package:
										$ npm install mysql

								2) We have to attempt connecting to the MySQL server with the correct credentials,
								   and picking a certain database:
										$ var mysql = require('mysql');
     
    										  var connection = mysql.createConnection({
      											host     : 'localhost',
      											user     : 'root',     // your root username
      											database : 'join_us'   // the name of your db
    										  });

								3) We make our queries (here is one form of a query using Node):
										$ var q = 'SELECT CURTIME() as time, CURDATE() as date, NOW() as now';

    										  connection.query(q, function (error, results, fields) {
      											if (error) throw error;
      											console.log(results[0].time);
      											console.log(results[0].date);
      											console.log(results[0].now);
    										  });

	-Create our Users		Note: We create the table inside of mySQL (not Node):
	 Table:						$ CREATE TABLE users (
        								email VARCHAR(255) PRIMARY KEY,
        								created_at TIMESåTAMP DEFAULT NOW()
    							  );

	-Selecting Using			Syntax:		* Given that we added users using MySQL we:
	 Node:
								* To select all Users from a database:

	 							  	$ var q = 'SELECT * FROM users ';
    									  connection.query(q, function (error, results, fields) {
      										if (error) throw error;
      										console.log(results);
    									  });

								* To count the number of users in the database:
									$ var q = 'SELECT COUNT(*) AS total FROM users ';
    									  connection.query(q, function (error, results, fields) {
      										if (error) throw error;
      										console.log(results[0].total);
    									  });

	-Insert Using 			Syntax:		* We could do it with a hardcoded string query (q) just like everything else we've done.
	 Node:						  BUT, to do it dynamically we can do it like so:
 
    								$ var person = {
        									email: faker.internet.email(),
        									created_at: faker.date.past()
    								  };
     
    								  var end_result = connection.query('INSERT INTO users SET ?', person, function(err, result) {
      									if (err) throw err;
      									console.log(result);
    								  });

							** Here if we pass the object, depending on the order, the "?" Reads the object property orders, and 
							   allows the object to be passed properly

	-Bulk inserting 500
	 Users using Node:		Syntax:		$ var mysql = require('mysql');
    							  var faker = require('faker');
     
     
    							  var connection = mysql.createConnection({
      								host     : 'localhost',
      								user     : 'root',
      								database : 'join_us'
    							  });
     
     
    							  var data = [];
    							  for(var i = 0; i < 500; i++){
        								data.push([
            								faker.internet.email(),
            								faker.date.past()
        								]);
    							  }
     
     
    							  var q = 'INSERT INTO users (email, created_at) VALUES ?';
     
    							  connection.query(q, [data], function(err, result) {
      								console.log(err);
      								console.log(result);
    							  });
     
    							  connection.end();

	