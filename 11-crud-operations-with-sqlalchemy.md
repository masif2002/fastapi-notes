# CRUD Operations with sqlalchemy

Anytime we need to perform any kind of database operation with sqlalchemy, we pass the following as a parameter into our path operation
```python
db: Session = Depends(get_db)
```
* To make a query we are going to make use of this database object
    ```
    post = db.query(models.Post).all()
    ```
    * Inside query(), we pass in the specific model for the table we want to query. 
    * Since, we need to query all of our entries, we use **all()**.
* The `db.query(models.Post)` statement actually contains only the underlying SQL statment that is required to retrieve the entries. You can print this statement to have a look at the SQL statements that sqlalchemy has prepared to retrieve the entries.
* By mentioning `.all()`, we actually execute the **query** for which the results are stored in the variable _post_.

## Adding a new entry
```python
new_post = models.Post(title=post.title, content=post.content, published=post.published)
db.add(new_post)
db.commit()
```
* `db.add()` is used to add a new entry to the table
* And like we did ealier, we need to commit to the database once we've made any changes.
* Instead of hardcoding each of the fields when creating the new post object, we can unpack our _post_ dictionary that comes from the Pydantic model
    ```python
    new_post = models.Post(**post.dict())
    ```
    * `**post.dict()` unpacks all the dictionary items. Read more about it [here](https://stackoverflow.com/questions/21809112/what-does-tuple-and-dict-mean-in-python).

## filter()
```python
post = db.query(models.Post).filter(models.Post.id == id).first()
```
* We use this to filter our results. It is similar to using WHERE in sql commands.
* Here, we use `.first()` to get the first result. because we know that, there is only going to be one post with the given id. We could use `.all()` but, once it finds the post with the id and it does not stop, it keeps on searching to see if there's any other row with the same id. So using `all()` becomes inefficient here.

## Deleting an entry
```python
db.query(models.Post).filter(models.Post.id == id).delete(synchronize_session=False)
```

## Updating an entry
```python
db.query(models.Post).filter(models.Post.id == id).update(post.dict(), synchronize_session=False)
```
* A dictionary needs to be passed inside the update() method to update the values.
* When updating and deleting an entry, make sure to commit the changes to the database.

## Sample path operations for reference
```python
@app.get("/posts")
def get_posts(db: Session = Depends(get_db)):

    posts = db.query(models.Post).all()
    
    return {"data": posts}

@app.post("/posts", status_code=status.HTTP_201_CREATED) 
def create_post(post: Post, db: Session = Depends(get_db):
    #new_post = models.Post(title=post.title, content=post.content, published=post.published, )
    new_post = models.Post(**post.dict()) # Unpacks the dictionary and sets in the format as the line above
    db.add(new_post)
    db.commit()
    db.refresh(new_post) # Retrievex the new post and store it into new_post. Similar to RETURNING * in sql

    return {"data": new_post}

@app.get("/posts/{id}")
def get_post(id: int, db: Session = Depends(get_db)):

    post = db.query(models.Post).filter(models.Post.id == id).first()

    if not post: 
           raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail=f"Post with id: {id} not found!")

    return {"Post": post}

@app.delete("/posts/{id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_post(id: int, db: Session = Depends(get_db)):
    
    post_query = db.query(models.Post).filter(models.Post.id == id)
    post = post_query.first()

    if not post: 
           raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail=f"Post with id: {id} not found!")

    # If post exists, we delete it
    post_query.delete(synchronize_session=False)
    db.commit()

    return Response(status_code=status.HTTP_204_NO_CONTENT)

@app.put("/posts/{id}")
def update_post(id: int, updated_post: Post, db: Session = Depends(get_db)): 

    post_query = db.query(models.Post).filter(models.Post.id == id)
    post = post_query.first()

    if not post:
        raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail=f"Post with id: {id} not found!")
        
    # If post exists
    post_query.update(updated_post.dict(), synchronize_session=False)
    db.commit()
    db.refresh(post)

    return {"updated_post": post}
```

## label()
While querying the database and selecting the column, if we need to alias our column name just like we do it with the **AS** keyword in sql, we need to use `label()` in sqlalchemy
```py
results = db.query(models.Post.id.label('id'), models.Post.title.label('title')).all()
```
## func - COUNT, MAX, MIN, SUM..
```python
from sqlalchemy.sql.functions import func

results = db.query(models.Post, func.count(models.Vote.post_id).label('votes')).group_by(models.Post.id).all()   
```
* The sql functions such as COUNT, MAX, SUM and many more can be accessed by importing **func** from sqlalchemy.