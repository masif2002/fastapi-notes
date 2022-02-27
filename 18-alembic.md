# Alembic
We know that sqlalchemy has a limitation when it comes to building tables and the database schema. SQLAlchemy does not allow us to modify the tables once they'been created. 
* **Alembic** is a lightweight database migration tool for usage with the SQLAlchemy Database Toolkit for Python.
* A **database migration tool** allows us to incrementally track changes to database schema and rollback changes to any point in time, just like we track changes in our code using _git_.

## Commands
```
alembic --help 
```
* Returns all the commands that you can use with alembic

## Initialisation
* First, we install alembic using the pip command.
    ```
    pip install alembic
    ```
* Then we initialise an alembic directory for us
    ```
    alembic init <name of the directory>
    ```
* Since alembic works with sqlalchemy and models that we build with sqlalchemy, we need to make sure it has access to our **Base** object from _database.py_ where we set up our database with sqlalchemy.
    * But, we need to import this Base object from _models.py_. It is technically the same object from _database.py_ BUT importing it from _models.py_ will allow alembic to read all of the models that we defined.
    * So we need to import it in **env.py** which is inside the alembic directory and is the main configuration file. After importing it into _env.py_, we do the following
    ```
    target_metadata = Base.metadata
    ```
    * 'Base' is the name of our base object that we imported
    * *target_metadata* is a variable in the file which is set to None by default
    * This action rigs it up with sqlalchemy.
* Next, we need to pass in the sqlalchemy url in **alembic.ini**
    ```
    sqlalchemy.url = postgres+psycopg2://user:pass@localhost:portno/dbname
    ```
    * Adding the driver after the + sign in the URL is optional which tells alembic to use the spcified driver or else it'll use the default driver.
    * However, instead of hardcoding this, we can use the Alembic Config object in **env.py** and set our database url
        ```
        config.set_main_option('sqlalchemy.url', 'postgres+psycopg2://user:pass@localhost:portno/dbname')
        ```
        * Here, we actually again are hardcoding in the exact URL, which we are doing just for demonstration purpose. But here, you need to actually import the __settings__ object we created in _config.py_ which we saw in the previous chapters (environment-variables) and pass those variables in an f string.
    * With this, we overwrite the sqlalchemy url we had in the **alembic.ini** file.

## Revision
* When we want to make a change to our database we create a revision.
    ```
    alembic revision -m "create posts table"
    ```
    * Using the -m flag, we are allowed pass in a title message for the revision being created. 
* The versions folder in the alembic driectory is going to contain all of our changes (or revisions).
* In the revision file, there are two functions - upgrade() and downgrade(). All of the logic for performing the change, for example, creating a table. So, the logic for creating the table goes into the **upgrade()** function. And the logic for undoing the changes goes into **downgrade()** function. The reason for this is that, if we ever needed to roll back to a previous revision, the downgrade() function is run thereby undoing the changes.
* So after completing the logic to perform the operation, we need to run the upgrade command which actually performs the operation.
    ```
    alembic upgrade 88fb11965e20    
    ```
    * We need to pass in the revision id to the upgrade command which will present in each revision file
    * We also can see what all are the required arguments with each command using --help
        ```
        alembic upgrade --help
        ```
        * This returns all the positional and optional arguments that can be used along with the _upgrade_ command. Similarly --help can be use with any command.
* So like this, we keep on creating revisions for every change we like to make to our database.
---
## Sample revision file
```py
"""add users table

Revision ID: 88fb11965e20
Revises: 043bdf6fcef1
Create Date: 2022-01-17 20:37:16.951993

"""
from alembic import op
from sqlalchemy import *


# revision identifiers, used by Alembic.
revision = '88fb11965e20'
down_revision = '043bdf6fcef1'
branch_labels = None
depends_on = None


def upgrade():
    op.create_table('users', 
    Column('id', Integer, nullable=False),
    Column('email', String, nullable=False),
    Column('password', String, nullable=False),
    Column('created_at', TIMESTAMP(timezone=True),nullable=False, server_default=text('now()')),
    PrimaryKeyConstraint('id'),
    UniqueConstraint('email')
    )


def downgrade():
   op.drop_table('users')
```
* Various database operations like create table, add column, with alembic is defined [here](https://alembic.sqlalchemy.org/en/latest/api/ddl.html) in the documentation.

## --autogenerate with revision
We have an `--autogenerate` flag that we can use when we create a revision
* This automatically generates all the logic for the revision by comparing it with our existing database models that we created in _database.py_. 
* And, if we've already have previous revisions with alembic, it only checks for what is missing and then adds it