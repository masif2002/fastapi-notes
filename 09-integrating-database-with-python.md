# Integration of database in Python
We use a library called **psycopg2**, with which we'll be integrating the postgres database into our python program.

## Intializing connection to the database
```python
import psycopg2
from psycopg2.extras import RealDictCursor

try:
    conn = psycopg2.connect(host='localhost', database='fastapi', user='postgres', password=1234, cursor_factory=RealDictCursor)
    print("Database connected Successfully!")
except Exception as e:
    print("Database Connection Failed!")
    print("Error:", e)
```
* Here, _fastapi_ is the name of our database that we created in postgres.
* We import **RealDicrCursor** and make use of it to specify which value maps to which column when retreiving entries in our database. **psycopg** does not do it by default, so we have to pass it in as a parameter when establishing a connection.
* And while establishing a connection, it is highly recommended to do it inisde a try-except block because any type of error may occur which would result in the failure of the database connection.

## Creating a cursor
We need to open a **cursor** to perform database operations.
```python
cursor = conn.cursor()
```
* This cursor object will be used to execute any commands to perform database operation later in the program.

## Executing SQL queries
```python
cursor.execute("SELECT * FROM posts;")
```
* This is how any SQL query is executed in python.
* The actual command is passed in double quotes inside the prantheses.

We have executed the sql command, but we need a way to **fetch** the results. We can do so by
```python
posts = cursor.fetchall()
print(posts)
```
* This fetches the results once the sql query is executed. There is also **fetchone()** and **fetchmany()** which also can be used depending upon how many results to fetch.
## Committing changes to the database
```python
def create_post(post: Post):
    cursor.execute("INSERT INTO posts (title, content, published) VALUES (%s, %s, %s) RETURNING *;", (post.title, post.content, post.published))

    new_post = cursor.fetchone()
    conn.commit()
    return {"data": new_post}
```
* Here, we execute the command to insert a new entry into the table.
* We could've passed an f string just like we used to do previously. We still could do that but it makes us vulnerable to **SQL injection**.
    * _SQL injection_ usually occurs when the user gives you an SQL statement instead of the input you expect that will unknowingly run on your database and provide the user information which may not be meant for them.
    * So to avoid that, we use a **%s** which acts a placeholder for the actual values we need to pass, which is passed in a tuple after the string seperated by a comma.
        * If only a single value is passed in a tuple, make sure to leave a comma after the value, because Python won't recognize it as a tuple.
* Here, we use `cursor.fetchone()` because we know that we are only creating ONE post. So there will be ONE row that is going to be returned.
* Another important point is that we need to **commit** the changes to the database whenever we make a change.
    ```
    conn.commit()
    ```
> Documentation of psycopg2:
https://www.psycopg.org/docs/usage.html
