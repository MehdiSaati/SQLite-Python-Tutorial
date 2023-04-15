# SQLite-Python-Tutorial
![SQLite-Python](https://user-images.githubusercontent.com/117528133/231989116-846ffa59-424a-4dec-a54f-f25986e036e7.jpg)

This section shows you step by step how to work with the SQLite database using Python programming language.

Python provides two popular interfaces for working with the SQLite database library: PySQLite and APSW. Each interface targets a set of different needs.

PySQLite
The PySQLite provides a standardized Python DBI API 2.0 compliant interface to the SQLite database. If your application needs to support not only the SQLite database but also other databases such as MySQL, PostgreSQL, and Oracle, the PySQLite is a good choice.

PySQLite is a part of the Python standard library since Python version 2.5

APSW
If your application needs to support only the SQLite database, you should use the APSW module, which is known as Another Python SQLite Wrapper.

The APSW provides the thinnest layer over the SQLite database library. The APSW is designed to mimic the native SQLite C, therefore, whatever you can do in SQLite C API, you can do it also from Python.

Besides covering the SQLite library, the APSW provides many low-level features including the ability to create user-defined aggregate, function, and collations from Python. It even allows you to write a virtual table implementation using Python.

#SQLite Python: Creating a New Database
Summary: in this tutorial, you will learn how to create a new SQLite database from a Python program.

When you connect to an SQLite database file that does not exist, SQLite automatically creates the new database for you.

To create a database, first, you have to create a Connection object that represents the database using the connect() function of the sqlite3 module.

For example, the following Python program creates a new database file pythonsqlite.db in the c:\sqlite\db folder.
```
Note that you must create the c:\sqlite\db folder first before you execute the program. Or you can place the database file a folder of your choice.
```

```
import sqlite3
from sqlite3 import Error


def create_connection(db_file):
    """ create a database connection to a SQLite database """
    conn = None
    try:
        conn = sqlite3.connect(db_file)
        print(sqlite3.version)
    except Error as e:
        print(e)
    finally:
        if conn:
            conn.close()


if __name__ == '__main__':
    create_connection(r"C:\sqlite\db\pythonsqlite.db")

```

In this code:

First, we define a function called create_connection() that connects to an SQLite database specified by the database file db_file. Inside the function, we call the connect() function of the sqlite3 module.

The connect() function opens a connection to an SQLite database. It returns a Connection object that represents the database. By using the Connection object, you can perform various database operations.

In case an error occurs, we catch it within the try except block and display the error message. If everything is fine, we display the SQLite database version.

It is a good programming practice that you should always close the database connection when you complete with it.

Second, we pass the path of the database file to the create_connection() function to create the database. Note that the prefix r in the  r"C:\sqlite\db\pythonsqlite.db" instructs Python that we are passing a raw string.

Let’s run the program and check the c:\sqlite\db folder.
If you skip the folder path c:\sqlite\db, the program will create the database file in the current working directory (CWD).

If you pass the file name as :memory: to the connect() function of the sqlite3 module, it will create a new database that resides in the memory (RAM) instead of a database file on disk.

The following program creates an SQLite database in the memory.

```
import sqlite3
from sqlite3 import Error


def create_connection():
    """ create a database connection to a database that resides
        in the memory
    """
    conn = None;
    try:
        conn = sqlite3.connect(':memory:')
        print(sqlite3.version)
    except Error as e:
        print(e)
    finally:
        if conn:
            conn.close()


if __name__ == '__main__':
    create_connection()
```

In this tutorial, you have learned how to create an SQLite database on disk and in memory from a Python program using sqlite3 module.

###SQLite Python: Creating Tables
Summary: in this tutorial, we will show you how to create tables in the SQLite database from the Python program using the sqlite3 module.

To create a new table in an SQLite database from a Python program, you use the following steps:

First, create a Connection object using the connect() function of the sqlite3 module.
Second, create a Cursor object by calling the cursor() method of the Connection object.
Third, pass the CREATE TABLE statement to the execute() method of the Cursor object and execute this method.

The following CREATE TABLE statements create these two tables:
```
-- projects table
CREATE TABLE IF NOT EXISTS projects (
	id integer PRIMARY KEY,
	name text NOT NULL,
	begin_date text,
	end_date text
);

-- tasks table
CREATE TABLE IF NOT EXISTS tasks (
	id integer PRIMARY KEY,
	name text NOT NULL,
	priority integer,
	project_id integer NOT NULL,
	status_id integer NOT NULL,
	begin_date text NOT NULL,
	end_date text NOT NULL,
	FOREIGN KEY (project_id) REFERENCES projects (id)
);
Code language: SQL (Structured Query Language) (sql)
```

Let’s see how to create new tables in Python.

First, develop a function called create_connection() that returns a Connection object which represents an SQLite database specified by the database file parameter db_file.
```
def create_connection(db_file):
    """ create a database connection to the SQLite database
        specified by db_file
    :param db_file: database file
    :return: Connection object or None
    """
    conn = None
    try:
        conn = sqlite3.connect(db_file)
        return conn
    except Error as e:
        print(e)

    return conn
```

Second, develop a function named create_table() that accepts a Connection object and an SQL statement. Inside the function, we call the execute() method of the Cursor object to execute the CREATE TABLE statement.
```
def create_table(conn, create_table_sql):
    """ create a table from the create_table_sql statement
    :param conn: Connection object
    :param create_table_sql: a CREATE TABLE statement
    :return:
    """
    try:
        c = conn.cursor()
        c.execute(create_table_sql)
    except Error as e:
        print(e)
Code language: SQL (Structured Query Language) (sql)
```

Third, create a main() function to create the  projects and tasks tables.
```
def main():
    database = r"C:\sqlite\db\pythonsqlite.db"

    sql_create_projects_table = """ CREATE TABLE IF NOT EXISTS projects (
                                        id integer PRIMARY KEY,
                                        name text NOT NULL,
                                        begin_date text,
                                        end_date text
                                    ); """

    sql_create_tasks_table = """CREATE TABLE IF NOT EXISTS tasks (
                                    id integer PRIMARY KEY,
                                    name text NOT NULL,
                                    priority integer,
                                    status_id integer NOT NULL,
                                    project_id integer NOT NULL,
                                    begin_date text NOT NULL,
                                    end_date text NOT NULL,
                                    FOREIGN KEY (project_id) REFERENCES projects (id)
                                );"""

    # create a database connection
    conn = create_connection(database)

    # create tables
    if conn is not None:
        # create projects table
        create_table(conn, sql_create_projects_table)

        # create tasks table
        create_table(conn, sql_create_tasks_table)
    else:
        print("Error! cannot create the database connection.")
```
Fourth, execute the main() function.
```
if __name__ == '__main__':
    main()
 ```
 Here is the full program:
 ```
import sqlite3
from sqlite3 import Error


def create_connection(db_file):
    """ create a database connection to the SQLite database
        specified by db_file
    :param db_file: database file
    :return: Connection object or None
    """
    conn = None
    try:
        conn = sqlite3.connect(db_file)
        return conn
    except Error as e:
        print(e)

    return conn


def create_table(conn, create_table_sql):
    """ create a table from the create_table_sql statement
    :param conn: Connection object
    :param create_table_sql: a CREATE TABLE statement
    :return:
    """
    try:
        c = conn.cursor()
        c.execute(create_table_sql)
    except Error as e:
        print(e)


def main():
    database = r"C:\sqlite\db\pythonsqlite.db"

    sql_create_projects_table = """ CREATE TABLE IF NOT EXISTS projects (
                                        id integer PRIMARY KEY,
                                        name text NOT NULL,
                                        begin_date text,
                                        end_date text
                                    ); """

    sql_create_tasks_table = """CREATE TABLE IF NOT EXISTS tasks (
                                    id integer PRIMARY KEY,
                                    name text NOT NULL,
                                    priority integer,
                                    status_id integer NOT NULL,
                                    project_id integer NOT NULL,
                                    begin_date text NOT NULL,
                                    end_date text NOT NULL,
                                    FOREIGN KEY (project_id) REFERENCES projects (id)
                                );"""

    # create a database connection
    conn = create_connection(database)

    # create tables
    if conn is not None:
        # create projects table
        create_table(conn, sql_create_projects_table)

        # create tasks table
        create_table(conn, sql_create_tasks_table)
    else:
        print("Error! cannot create the database connection.")


if __name__ == '__main__':
    main()
```
 Let’s verify if the program has created those tables successfully in the pythonsqlite.db database.

First, launch the command line and connect to the  pythonsqlite.db database:

>sqlite3 c:\sqlite\db\pythonsqlite.db

```
>sqlite3 c:\sqlite\db\pythonsqlite.db
```
Then, use the .tables command to display the tables in the database.
```
sqlite> .tables
projects  tasks
```
As you can see clearly from the output, we are having the projects and tasks tables in the pythonsqlite.db database. And the program works as expected.

In this tutorial, you have learned how to create new tables in the SQLite database using the execute() method of the Cursor object.
