# SQLAlchemy

## Object Relational Mapper (ORM)
We previously saw how we can use the default postgres driver - **psycopg2** to interact with a postgres database by sending SQL commands.

Howebver, there are other methods to do the same. One of the popular method ways of working with databases is by using an **ORM**. 
* With an ORM, we don't interact with the database directly,  instead we talk to the ORM which will actually interact with the database.
* The benefit of working with an ORM is that we don't have to work with SQL anymore. So instead of using raw SQL commands, we use standard python code calling various functions and methods that alternately turns into SQL themseleves.
* When using an ORM, we define our tables as Python **models** with standard python classes instead of creating a table in pgAdmin for postgres like we did before.

**SQLAlchemy** is one of the most popular python ORMs. It is a standalone library and can be used with any other python web frameworks or any python based application/

## Setting up sqlalchemy
* First we need to install the library using the pip command. The version that is being used currently is 1.4.x
* We basically follow the FastAPI documentation to setup sqlalchemy which can be found [here](https://fastapi.tiangolo.com/tutorial/sql-databases/). It is pretty straighforward.

### database.py
```python
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

SQLALCHEMY_DATABASE_URL = "postgresql://postgres:1234@localhost/fastapi"

engine = create_engine(SQLALCHEMY_DATABASE_URL)

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()
```
* *database.py* is the file that is going to handle the database connection.
* The syntax for assigning the url to `SQLALCHEMY_DATABASE_URL` is 
    ```
    postgresql://<username>:<password>@<ip-address/hostname>/<database_name>
    ```
* The **engine** is responsible for sqlalchemy to connect to postgres.
* Each instance of the **SessionLocal** class will be a database session. The class itself is not a database session yet but it will be later imported to _main.py_ and instantiated.
* We create the **Base** class, which we will later inherit from to create each of the database models or classes (the ORM models).

Then we create _models.py_ which is going to store all our models. Every model represents a table in our database. Creating models can also be followed along in the documentation. 
### models.py
```python
from sqlalchemy import Boolean, Column, Integer, String
from sqlalchemy.sql.expression import text
from sqlalchemy.sql.sqltypes import TIMESTAMP
from .database import Base

class Post(Base):
    __tablename__ = "posts"

    id = Column(Integer, primary_key=True, nullable=False)
    title = Column(String, nullable=False)
    content = Column(String, nullable=False)
    published = Column(Boolean, server_default='True', nullable=False) #Server_default is the default value for the column
    created_at = Column(TIMESTAMP(timezone=True),nullable=False, server_default=text('now()'))
```
The **Main FastAPI app** section in the documentation guides us how to import our database files and start a session in _main.py_.

## Limitation in sqlalchemy

* After the models have been created, imported in the main file and when the server is run, SQLAlchemy searches for the models. If it exists, it ignores or else it creates a new one.
* So if we had to make any change in our model, like to add an extra column, that wouldn't be reflected in our database once the table has been already created because it sees that there's a table with the given name already and it ignores and moves on. So this is a limitation in sqlalchemy.
* If you need to actually change it, we need to go to postgres and delete the existing table, and then rerun the server which creates the models with the new changes.

## Pydantic model vs SQLAlchemy model
Pydantic models are used for defining the shape/strucure of requests and responses whereas an SQLAlchemy model is responsible for defining the columns of our _posts_ table within postgres. It is used to query, create, delete and update entries within the database.