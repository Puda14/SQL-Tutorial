# SQL-Tutorial
SQL Server

## Create Database
Create a database using Transact-SQL, as shown below:
```sh
USE master;
GO
CREATE DATABASE nameDB
ON
( NAME = nameDB_dat,
FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.SQLEXPRESS\MSSQL\DATA\nameDBdat.mdf',
SIZE = 10,
MAXSIZE = 50,
FILEGROWTH = 5 )
LOG ON
( NAME = nameDB_log,
FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.SQLEXPRESS\MSSQL\DATA\nameDBlog.ldf',
SIZE = 5MB,
MAXSIZE = 25MB,
FILEGROWTH = 5MB );
GO
```
> Note the items that need to be modified:
> - Database name `nameDB`
>- File path link `Filename`
## Create Table
### Template
```sh
CREATE TABLE table_name (
column1 datatype,
column2 datatype,
column3 datatype,
....
);
```
### Example
```sh
CREATE TABLE Person(
PersonID int,
LastName varchar(255),
FirstName varchar(255),
Age int,
Gender char(1),
City varchar(255)
);
```
### Some SQL data types include
- `INT`: Integer data type, for example: INT, INTEGER.
- `VARCHAR`: Variable-length character string, for example: VARCHAR(255).
CHAR: Fixed-length character string, for example: CHAR(10).
> NOTE: if you want to use Vietnamese, use `nvarchar(...)` and when you insert, use: `N'String'`
- `DECIMAL`/NUMERIC: Exact numeric data types, used for storing precise decimal numbers.
- `FLOAT`/REAL: Floating-point numeric data types, used for approximate decimal numbers.
- `DATE`: Date data type, stores dates without time.
- `TIME`: Time data type, stores time without date.
- `DATETIME`/TIMESTAMP: Date and time data type, stores both date and time information.
- `BOOLEAN`: Boolean data type, represents true or false values.

### INSERT
```sh
INSERT INTO Person (PersonID, LastName, FirstName, Age, Gender, City)
VALUES
    (1, 'Smith', 'John', 25, 'M', 'New York'),
    (2, 'Johnson', 'Emily', 32, 'F', 'Los Angeles'),
    (3, 'Williams', 'Michael', 41, 'M', 'Chicago'),
    (4, 'Brown', 'Jennifer', 28, 'F', 'Houston'),
    (5, 'Jones', 'Robert', 36, 'M', 'San Francisco'),
```

### DROP TABLE
The `DROP TABLE` statement is used to drop an existing table in a
database.
```sh
DROP TABLE table_name;
```
### TRUNCATE TABLE
The `TRUNCATE` TABLE statement is used to delete the data inside a table, but not the
table itself.
```sh
TRUNCATE TABLE table_name;
```
### ALTER TABLE
- The `ALTER TABLE` statement is used to add, delete, or modify columns in an
existing table.

- The `ALTER TABLE` statement is also used to add and drop various constraints on an existing table.

ALTER TABLE - `ADD Column`
```sh
ALTER TABLE table_name ADD column_name datatype;
```
ALTER TABLE - `DROP COLUMN`
```sh
ALTER TABLE table_name DROP COLUMN column_name;
```
ALTER TABLE - `ALTER/MODIFY COLUMN`
```sh
ALTER TABLE table_name ALTER COLUMN column_name datatype;
```
### SQL Constraints
- `NOT NULL` - Ensures that a column cannot have a NULL value
- `UNIQUE` - Ensures that all values in a column are
different
- `PRIMARY KEY` - A combination of a NOT NULL and
UNIQUE. Uniquely identifies each row in a table
- `FOREIGN KEY` - Prevents actions that would destroy links between tables
- `CHECK` - Ensures that the values in a column satisfies a specific condition
- `DEFAULT` - Sets a default value for a column if no value is specified
- `CREATE INDEX` - Used to create and retrieve data from the database very quickly
Template:
```sh
CREATE TABLE table_name (
column1 datatype constraint,
column2 datatype constraint,
column3 datatype constraint,
....
);
```
### EXAMPLE SQL Constraints
#### **NOT NULL**
```sh
DROP TABLE Person
CREATE TABLE Person(
PersonID int NOT NULL,
LastName varchar(50) NOT NULL,
FirstName varchar(50) NOT NULL,
Age int,
Gender char(1),
City varchar(255)
);
```
- Use **ALTER**
```sh
ALTER TABLE Person ALTER COLUMN Age int NOT NULL;
```

#### **UNIQUE**
```sh
CREATE TABLE Person(
PersonID int NOT NULL UNIQUE,
...
);
```
- Use CONSTRAINT
```sh
CREATE TABLE Person(
PersonID int NOT NULL,
LastName varchar(50) NOT NULL,
FirstName varchar(50) NOT NULL,
...
CONSTRAINT UC_Person UNIQUE (PersonID,LastName)
);
```
- Use ALTER
```sh
ALTER TABLE Person ADD CONSTRAINT UC_Person UNIQUE (PersonID,LastName)
```
- DROP UNIQUE CONSTRAINT ?
```sh
ALTER TABLE Person DROP CONSTRAINT UC_Person;
```
#### **PRIMARY KEY**
```sh
CREATE TABLE Person(
PersonID int NOT NULL PRIMARY KEY,
...
);
```
- Use PRIMARY KEY(...)
```sh
CREATE TABLE Person(
PersonID int NOT NULL,
...
PRIMARY KEY(PersonID),
...
);
```
- Use CONSTRAINT
```sh
CREATE TABLE Person(
PersonID int NOT NULL,
LastName varchar(50) NOT NULL,
...
CONSTRAINT PK_Person PRIMARY KEY(PersonID,LastName)
);
```
- Use ALTER
```sh
ALTER TABLE Person ADD PRIMARY KEY(PersonID);
```
>NOTE: The primary key has not been named !!!
- To allow naming of a `PRIMARY KEY`
constraint, and for defining a PRIMARY KEY constraint on multiple columns, use the
following SQL syntax:
```sh
ALTER TABLE Person ADD CONSTRAINT PK_Person PRIMARY KEY(PersonID,LastName);
```
- DROP PRIMARY KEY ?
```sh
ALTER TABLE Person DROP CONSTRAINT PK_Person;
```
#### **FOREIGN KEY**
```sh
CREATE TABLE Person(
PersonID int NOT NULL PRIMARY KEY,
...
);

CREATE TABLE Orders (
OrderID int NOT NULL PRIMARY KEY,
OrderNumber int NOT NULL,
PersonID int FOREIGN KEY REFERENCES Person(PersonID)
);
```
- Use CONSTRAINT
```sh
CREATE TABLE Orders(
OrderID int NOT NULL,
...
PRIMARY KEY(OrderID),
...
CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID) REFERENCES Person(PersonID)
);
```
- Use ALTER
```sh
ALTER TABLE Orders ADD FOREIGN KEY (PersonID) REFERENCES Person(PersonID);
```
>NOTE: The `FOREIGN KEY` has not been named !!!
- To allow naming of a FOREIGN KEY constraint, and for defining a FOREIGN KEY constraint on multiple columns, use the following SQL syntax:
```sh
ALTER TABLE Orders ADD CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID) REFERENCES Person(PersonID);
```
- DROP PRIMARY KEY ?
```sh
ALTER TABLE Orders DROP CONSTRAINT FK_PersonOrder;
```
#### **CHECK**
```sh
CREATE TABLE Person(
PersonID int NOT NULL PRIMARY KEY,
...
Age int CHECK (Age>=1 AND Age<=130),
...
);
```
- Use CONSTRAINT
```sh
CREATE TABLE Person(
PersonID int NOT NULL PRIMARY KEY,
...
Age int,
City varchar(255),
CONSTRAINT CHK_Person CHECK (Age>=1 AND City='Florida')
);
```
- Use ALTER
```sh
ALTER TABLE Person ADD CHECK (Age>=1);
```
```sh
ALTER TABLE Person ADD CONSTRAINT CHK_PersonAge CHECK (Age>=1 AND City='Florida');
```
- DROP CHECK
```sh
ALTER TABLE Person DROP CONSTRAINT CHK_PersonAge;
```
### **DEFAULT**
```sh
CREATE TABLE Person(
PersonID int NOT NULL PRIMARY KEY,
...
City varchar(255) DEFAULT 'California'
);
```
- The DEFAULT constraint can also be used to insert system values, by using functions like GETDATE():
```sh
CREATE TABLE Orders(
...
OrderDate date DEFAULT GETDATE()
);
```
- Use ALTER
```sh
ALTER TABLE Person ADD CONSTRAINT df_CityDEFAULT 'California' FOR City;
```
- DROP DEFAULT
```sh
ALTER TABLE Person DROP CONSTRAINT df_City
```
### **AUTO INCREMENT**
```sh
CREATE TABLE Person(
PersonID int identity(1,1) PRIMARY KEY,
...
);
```
>NOTE: IDENTITY[(`seed`, `increment`)]
## SELECT