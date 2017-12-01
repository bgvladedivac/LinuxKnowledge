## Relational database
A relational database is a digital database based on the relational model of data. A software system used to maintain relational databases is a relational database management system (**RDBMS**). Virtually all relational database systems use *SQL (Structured Query Language)* for querying and maintaining the database.

This model organizes data into one or more tables (or "relations") of columns and rows, with a unique key identifying each row. Rows are also called records. Columns are also called attributes. Generally, each table/relation represents one "entity type" (such as customer or product). The rows represent instances of that type of entity (such as "Lee" or "chair") and the columns representing values attributed to that instance (such as address or price).

Each row in a table has its own unique key. Rows in a table can be linked to rows in other tables by adding a column for the unique key of the linked row (such columns are known as foreign keys). 

## Database normalization
Database normalization is the process of organizing the columns (attributes) and tables (relations) of a relational database to reduce data redundancy and improve data integrity. Normalization is also the process of simplifying the design of a database so that it achieves the optimal structure composed of atomic elements.

## MariaDB
A community-developed branch of MySQL built by some of the original authors of MySQL. 

## Installation Security
MariaDB provides a program to improve security from the baseline install state **mysql_secure_installation**.  This program enables improvement of MariaDB security by setting a password for root accounts, removing root accouts that accessible from the outside localhost, removes anonymous-user accounts and removes the test database.

The script is fully interactive, and will prompt for each step in the process.

## Creating a database 
The installation of the mariadb provides a program called **mysql**. With this program, it is possible to connect to a local or remote MariaDB database server. 

Connect to the MariaDB database server, specify the username, the host name and prompt for the password.
```{r, engine='bash', count_lines}
[root@server ~]# mysql -u root -h localhsot -p 
[root@server ~]# SHOW DATABASES; 
```



