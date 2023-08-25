# <strong>Oracle Database Cheat Sheet</strong>
<strong>RDBMS</strong> (Reletional Database Management System ) Using Oracle Database 19c.

-   <strong>DDL</strong> Data Definition Language
-   <strong>DML</strong> Data Manipulation Language
-   <strong>DRL</strong> Data Retrieval Language
-   <strong>DCL</strong> Data Control Language
-   <strong>TCL</strong> Data Control Language

<hr>

## <strong>DDL</strong> (Data Definition Language)
Attention: If you use this commands, that is not possible roll the changes back. This statements are auto commited and change meta data.

-   Create
-   Alter
-   Drop
-   Truncate
-   Rename

<hr>

## <strong>Create</strong> Statement
Creating a new table with some columns.
```sql
CREATE TABLE Members(
    MemberId NUMBER,
    Email VARCHAR2(64),
    Password VARCHAR2(64),
    Forename VARCHAR2(60),
    Surname VARCHAR2(140),
    CreationTime DATE DEFAULT SYSDATE
);

CREATE TABLE Test(
    TestId NUMBER
);
```
Showing creation description of created tabe.
```sql
DESC Members;
```

<h1>

## <strong>Alter</strong> Statement
Changing table definition of created table.

Adding one column.
```sql
ALTER Members ADD LastLoginTime DATE DEFAULT SYSDATE;
```

Adding multiple columns.
```sql
ALTER Members ADD(TempCol1 NUMBER(10,2), TempCol2 CHAR(32));
```

Change column definition.
```sql
ALTER Members MODIFY COLUMN Forename VARCHAR2(140);
/* 
    Attention:
        if the column type is VARCHAR2 and you want
        modify type to NUMBER, you may see ERROR, 
        if characters can't parse in NUMBERS.
*/
```

Delete a column.
```sql
ALTER Members DROP COLUMN TempCol1;
/*
    Attention:
        if the table have to much data and your
        will using drop, it may be to slow to drop.
        Because it will be remove this column from
        table and changes metadata and release used
        memory.

        Better way show the next statement.
*/
```
```sql
-- First set unused
ALTER TABLE Members SET UNUSED COLUMN TempCol2;
-- Then later if you have more resources to 
-- handle it drop unused
ALTER TABLE Members DROP UNUSED COLUMNS;
```

<hr>

## <strong>Drop</strong> Statemenet
Removing the table.
```sql
DROP TABLE Test;
```

<hr>

## <strong>Truncate</strong> Statement
Removing all data from existing table without remove table.
```sql
TRUNCATE TABLE Members;
```

## <strong>Rename</strong> Statement
Renaming a table or column.
```sql
-- Change table name
ALTER TABLE Members RENAME TO Users;
-- Change column name
ALTER TABLE Users RENAME COLUMN Forename TO Firstname;
-- change column name
ALTER TABLE Users RENAME COLUMN Surname TO Lastname;
```