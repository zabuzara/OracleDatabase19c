# Oracle Database Cheat Sheet
RDBMS (Reletional Database Management System ) Using Oracle Database 19c.

-   DDL Data Definition Language
-   DML Data Manipulation Language
-   DRL Data Retrieval Language
-   DCL Data Control Language
-   TCL Data Control Language


## DDL (Data Definition Language)
Attention: If you use this commands, that is not possible roll the changes back. This statements are auto commited and change meta data.

-   Create
-   Alter
-   Drop
-   Truncate
-   Rename

## Create Statement
```sql
CREATE TABLE Members(
    MemberId number,
    Email varchar2(64),
    Password varchar2(64),
    Forename varchar2(60),
    Surname varchar2(140),
    CreationTime date default sysdate
);
```sql