# Query parameters
* Query parameters are the extra fields in the URL when which you add when you need to filter something.
* The query parameter starts after the `?` and if there are multiple query parameters, each query parameter is seperated by a `&`.

Example:
```
https://www.yelp.com/search?find_desc=Restaurants&find_loc=San+Francisco%2C+CA&ns=1
```
* Here, `find_desc`, `find_loc` and `ns` are the three query parameters that can be extracted from the above URL

## Setting up query parameters in our route
To add a query parameter for our route, we can just add another parameter in our path operation.
```python
@router.get("/", response_model=List[schemas.Post])
def get_posts(db: Session = Depends(get_db), current_user = Depends(oauth2.get_current_user), limit_posts: int = 3):
    posts = db.query(models.Post).limit(limit_posts).all()
    return posts
```
* Here, we added a query parameter called `limit` which defaults to 3 if not specified in the URL.
* Now, we can make use of this query parameter when accessing our route (which is '/posts' here)
    ```
    http://127.0.0.1:8000/posts?limit_posts=5
    ```
* The **limit()** method in the above command is equivalent to using the LIMIT keyword in sql, which limits the results retrieved to the value specified.
* And similarly we can also use the **offset()** method, equivalent to OFFSET in sql which skips spcified number of entries and displays the records after it.
* We can add another query parameter that skips certain number of posts, with the help of the _offset()_ method.
    ```
    http://127.0.0.1:8000/posts?limit_posts=3&skip_posts=6
    ```
* Another example of query parameter is to filter titles that have certain keywords
    ```python
    posts = db.query(models.Post).filter(models.Post.title.contains(search)).limit(posts_limit).offset(posts_skip).all()
    ```
    * Here, we filter the results from our table by checking if our title contains the keyword that the user specifies in the query parameter using the **contain()** method.

### /posts route
```python
def get_posts(db: Session = Depends(get_db), current_user = Depends(oauth2.get_current_user), 
posts_limit: int = 3, posts_skip: int = 0, search: str = ""):

    posts = db.query(models.Post).filter(models.Post.title.contains(search)).limit(posts_limit).offset(posts_skip).all()
    return posts
```
### URL with query parameter
```
http://127.0.0.1:8000/posts?search=beaches
```
* This will return all the posts that contains the keyword 'beaches' in the title
* If you need to use keywords that contain space in the URL, you can make use of `%20` to specify the space character in the URL
    ```
    http://127.0.0.1:8000/posts?search=best%20beaches
    ```