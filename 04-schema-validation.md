# Schema validation with Pydantic

**Pydantic** is the library that we are going to make use of, for defining a _schema_. With that schema, we are going to make sure that the frontend would send the data to the backend only in the form of that particular schema.
* The first step is to import _BaseModel_ from **pydantic**
* Then we create our model which inherits from _BaseModel_.
```python
class Post(BaseModel):
    title: str
    content: str

@app.post('/createpost')
def create_post(new_post: Post):
    print(new_post)
    return {"data": "New Post"}
```
* Here, we create a Model called Post with which we'll allow users to create posts with a title and content of type string.
* Then, we add the model in our path operation aswell. Now, it extracts the data from the POST method in the form of our new schema that we passed.
* With this, _FastAPI_ is automatically going to validate the data that it receives from the user, based on the pydantic model - Post, that we created.
    * Validation in the sense, it checks that all the fields of the schema are present with the required datatype. Else, it will throw an error.
* Now, it prints out the data which is extracted. We can also access the fields seperately with the dot operator.
    ```
    print(new_post.title)
    print(new_post.content)
    ```
* We also created an optional field called _published_. It is optional because, even if we don't provide the value, it defaults to True.
    ```python
    class Post(BaseModel):
        title: str
        content: str
        published: bool=True
    ```
* We can also create a completely optional field.
    ```python
    class Post(BaseModel):
        title: str
        content: str
        rating: Optional[int]=None  
    ```
    * Here, we create a completely optional field called _rating_. If the user does not specify rating, it is stored as _None_. However, we cannot use the default value like we used before for _published_, because an integer cannot have the value _None_. 
    * And **Optional** needs to be imported from _typing_ which vscode automatically does it for us in the autocompletion.
* When we actually extract the data from POST method and save it into the variable *new_post*, it actually stores it as a pydantic model. And each pydantic model has a specific method called **dict()** which converts the object into a Python dictionary.
```python
@app.post("/createpost")
def create_post(post: Post):
    print(post)
    print(post.dict())
    return {"data": "New Post"}
```