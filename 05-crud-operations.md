# CRUD Operations 
A CRUD based application should be able to 
* **C**reate
* **R**ead
* **U**pdate
* **D**elete things.

These are the 4 main functions of any CRUD based application.
* Updating data actually can be done using two different HTTP methods
    * **PUT** method 
    * **PATCH** method 
* While updating data using the **PUT** method, we need to pass in all the fields to the API, whereas with **PATCH** method, we send only the specific field which we want to change.

## Path Parameter
A path parameter is something that is embedded in a URL which a user can provide when accessing a particular route or an URL endpoint. For example:
```python
@app.get("/posts/{id}")
def get_post(id):
    print(id)
    return {"post_detail":"Here is post {id}}
```
* Here, the _id_ field represents a **path parameter**. 
* _FastAPI_ is automatically going to extract this _id_ and we can pass it right into our function.  

As we've seen earlier, the order of path operations is really important, especially when you have a path parameter. Because whe you have a path parameter it could result in accidentally matching requests that was meant for a different route.

### Validation of data
We can perform validation with _FastAPI_.
* Some kind of validation to ensure that whataver data is passed into the path parameter can be actually of the datatype we'd like. 
* We can do this by specifying the datatype for the parameter after a semicolon
```python
@app.get("/posts/{id}")
def get_post(id: int): #Validation
    print(id)
    return {"post_detail":"Here is post {id}}
```
* Here, we specify the _id_ that we receive must be an integer.

## Changing response status codes
We can manipuate the reponse in _FastAPI_.
* First, we need to import _Response_ from FastAPI and pass it in as a parameter in our path operation function.
* Then we can set the status code of the response object by accessing the *status_code* property
```python
@app.get("/posts/{id}")
def get_post(id: int, response: Response):
    post = find_post(id)
    if not post: 
        response.status_code = 404 

    return {"Post": post}
```

This is one way of changing the status code. However, there is a better way of doing it instead of harcoding the value of the status code.
* For this, we need to import _status_ from the FastAPI library.
* So, now instead of having to rememeber what to use, we can just type status followed by the dot, which has the complete list of HTTP codes
```python
@app.get("/posts/{id}")
def get_post(id: int, response: Response):
    post = find_post(id)
    if not post: 
        response.status_code = status.HTTP_404_NOT_FOUND 

    return {"Post": post}
```

## Raising HTTP Exception
In the previous] example, even though we send a 404, there is no error message that we send to the user regarding what has happened. We can do this by hardcoding a message and returning it when the post is not found. But with _FastAPI_, we can do it in a better way. ie- by raising a HTTP Exception
* To do that, we need to first import _HTTPException_ from the FastAPI library
* In this Exception, we can pass in the error code aswell as the error message so that we don't need to hardcode all the stuff we previously did.
```python
@app.get("/posts/{id}")
def get_post(id: int):
    post = find_post(id)
    if not post: 
        raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail=f"Post with id: {id} not found!")

    return {"Post": post}
```
* A 404 status code is usually returned when something is not found. So, here if we coudn't find the post that the user is looking for, we send a 404.
* Similarly we 
    * return a 201 if we actually create an entity using the POST http method
    * and return a 204 if we DELETE something using the DELETE method
* The list of other response codes can be found [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

By default, we only return a status code - 200 for anything operation we perform. But we need to update it.
* To change the default status code, we pass in another parameter called *status_code* inside our decorator and we set it to the approprtiate status code that needs to be returned for the operation performed.
```python
@app.post("/posts", status_code=status.HTTP_201_CREATED) 
def create_post(post: Post):
    post_dict = post.dict()
    post_dict['id'] = randrange(1, 100000)
    my_posts.append(post_dict)
    return {"data": post_dict}
 ```
 Similarly we send a 204 when we delete something.

```python
@app.delete("/posts/{id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_post(id: int):
    post = find_post(id)
    if not post: 
           raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail=f"Post with id: {id} not found!")

    my_posts.remove(post)
    return Response(status_code=status.HTTP_204_NO_CONTENT)
```
* While returning a 204, we are not allowed to send any content back. So, we simply just send a response. 