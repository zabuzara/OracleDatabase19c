# <strong>Oracle Database Cheat Sheet</strong>
## Reference: [Tutorialspoint](https://youtu.be/y2CICRujsVw?feature=shared)
Some table structures and definition are changed.

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

### <strong>Create</strong> Statement
Creating a new table with some columns.
```sql
CREATE TABLE Members(
    MemberId NUMBER,
    Email VARCHAR2(64),
    Password VARCHAR2(64),
    Forename VARCHAR2(60),
    Surname VARCHAR2(140),
    CreationTime DATE DEFAULT SYSDATE,
    GameID NUMBER
);

CREATE TABLE Test(
    TestId NUMBER
);
```
Showing creation description of created tabe.
```sql
DESC Members;
```

### <strong>Alter</strong> Statement
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

### <strong>Drop</strong> Statemenet
Removing the table.
```sql
DROP TABLE Test;
```

### <strong>Truncate</strong> Statement
Removing all data from existing table without remove table.
```sql
TRUNCATE TABLE Members;
```

### <strong>Rename</strong> Statement
Renaming a table or column.
```sql
-- Change table name
ALTER TABLE Members RENAME TO Users;
-- Change column name
ALTER TABLE Users RENAME COLUMN Forename TO Firstname;
-- change column name
ALTER TABLE Users RENAME COLUMN Surname TO Lastname;
```

### <strong>Show User Tables</strong>
```sql
SELECT table_name FROM user_tables ORDER BY tabe_name;
```
### <strong>Show all accessible tables for current user</strong>
```sql
SELECT table_name FROM all_tables ORDER BY table_name;

-- for specific owner
SELECT * FROM all_tables WHERE OWNER = 'OT' ORDER BY table_name;

-- all table in oracle
SELECT table_name FROM dba_tables;
```

<hr>

## <strong>Constraints</strong>
- Adding rules which data must be allowed.
- Constraint name is optional.
- Can be added at column level or table level
- can be enabled/disabled/dropped

If you want make combination constraints, you must use constraints at the table level.

### Type of <strong>Constraints</strong>
    -   NOT NULL
    -   UNIQUE
    -   PRIMARY KEY
    -   CHECK
    -   FOREIGN KEY

### <strong>NOT NULL</strong> Constraint
```sql
-- NOT NULL can add just at column level
CREATE TABLE Test(
    TestId NUMBER CONSTRAINT test_col_not_null NOT NULL,
    TestName VARCHAR2(30) NOT NULL,
    TestDesc VARCHAR2(150),
    TestRef VARCHAR2(64)
);
-- Check this
DESC Test;

-- NOT NULL with ALTER 
ALTER TABLE Test MODIFY COLUMN TestRef VARCHAR(32) CONSTRAINT testref_col_not_null NOT NULL;
-- Check this
DESC Test;
```

### <strong>UNIQUE</strong> Constraint
```sql
CREATE TABLE Test2(
    /* At column level */
    Col1 NUMBER CONSTRAINT test2_col1_unique UNIQUE, 
    Col2 NUMBER,
    Col3 NUMBER,
    /* Combinate unique constraint at table level */
    CONSTRAINT test2_unique UNIQUE(Col2, Col3)
);

-- Adding on existing table
ALTER TABLE Test ADD CONSTRAINT test_name_unique UNIQUE(TestName);
ALTER TABLE Test ADD CONSTRAINT test_ref_unique UNIQUE(TestRef);
```

### <strong>PRIMARY KEY</strong> Constraint
```sql
-- At the column level
CREATE TABLE Test3(
    Test3Id NUMBER CONSTRAINT test3_pk PRIMARY KEY
);
DESC Test3;
DROP TABLE Test3;

-- At the table level
CREATE TABLE Test3(
    Test3Id NUMBER,
    CONSTRAINT test3_pk PRIMARY KEY(Test3Id)
);
DESC Test3;
DROP TABLE Test3;

-- Add to existing table
CREATE TABLE Test3(
    Test3Id NUMBER
);
ALTER TABLE Test3 ADD CONSTRAINT test3_pk PRIMARY KEY(Test3Id);
DESC Test3;
DROP TABLE Test3;
```

### <strong>CHECK</strong> Constraint
```sql
-- At the column level
CREATE TABLE Test3(
    Col1 NUMBER(10) CONSTRAINT test3_col1_check CHECK(Col1 >= 10000),
    Gender CHAR(1)
);

-- Add to existing table
ALTER TABLE Test3 ADD CONSTRAINT test3_gender_check CHECK(Gender in('M', 'F', 'D'));
```

### <strong>FOREIGN KEY</strong> Constraint
```sql
CREATE TABLE Games(
    GameId NUMBER CONSTRAINT games_pk PRIMARY KEY
);

-- Add to existing Members table
ALTER TABLE Members ADD CONSTRAINT member_game_fk FOREIGN KEY(GameId) REFERENCES Games(GameId) ON DELETE CASCADE;
-- ON DELETE CASCADE removes references on deleting

ALTER TABLE Members ADD CONSTRAINT member_game_fk FOREIGN KEY(GameId) REFERENCES Games(GameId) ON DELETE SET NULL;
-- ON DELETE SET NULL sets reference column to null on deleting
```

```sql
-- Double foreign key at the table level for between table
CREATE TABLE Members_Games(
    MemberId NUMBER,
    GameId NUMBER,
    CONSTRAINT members_games_member_fk FOREIGN KEY(MemberId) REFERENCES Members(MemberId),
    CONSTRAINT members_games_game_fk FOREIGN KEY(GameId) REFERENCES Games(GameId),
);
```

### Showing existing constraints
Description of user_constraints table.
```sql
DESC user_constraints;
```

```sql
-- Table name must be in uppercase
SELECT constraint_name, constraint_type FROM user_constraints WHERE table_name='MEMBERS_GAMES';
```

```sql
DESC user_cons_columns;
```

```sql
-- Table name must be in uppercase
SELECT constraint_name, column_name, owner FROM user_cons_columns WHERE table_name='MEMBERS_GAMES';
```

### <strong>DISABLE</strong> Constraint
```sql
ALTER TABLE Test3 DISABLE CONSTRAINT test3_gender_check;
```

### <strong>ENABLE</strong> Constraint
```sql
/*
    Attention: 
        if after disabling the check constraint
        you put invalid data or somthing, that not
        allow by constraint on enabling, this command
        will be not enabled constraint because of
        invalid characters thats try to validate with
        check constraint.
*/
ALTER TABLE Test3 ENABLE CONSTRAINT test3_gender_check;
```

### <strong>DROP</strong> Constraint
```sql
ALTER TABLE Test3 DROP CONSTRAINT test3_gender_check;
```
