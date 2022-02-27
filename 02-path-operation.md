# Path Operation

Previously, we installed _FastAPI_, initalised the app and run the webserver. Here, we'll see how we actually inistialised the app.
```python
from fastapi import FastAPI

app = FastAPI()

@app.get('\')
def root():
    return {"message":"Hello World"}
```
* The last three lines of the code is referred to as the **path operation**. It is also referred to what is called a **route**, which is how we called it when working with Flask previously. They fundamentally mean the same thing.
* The _path operation_ is made up of two components. 
* The first component is going to be the _function_.
    * The name of the function here, doesn't matter but it is recommended to name the functions in _path operation_ to be as descriptive as possible. 
    * Inside the function, we are allowed to do any operation. For example, if a function is created for logging in a user, we do the necessary operations to do so inside the function. 
    * Whatever is returned from the function, is the data that gets sent back to the user. In the above example, we return a Python dictionary, which the _FastAPI_ converts it to **json**, which can be seen as the main universal language of APIs.
        * We use **json** to send data back and forth to an API 
* And the second component is the _decorator_.
    * The work of the decorator is to change the behaviour of the function to which we are going to apply the decorator.
    * So here, the decorator changes our function to an actual _path operation_, so that someone who wants to use our API can hit the particular endpoint.
    * After the name of our app, we mention the type of the HTTP method. Here, we use the GET method, to send a GET request to our API.
    * And finally, we mention the _path_ inside the parantheses. This is the route URL.

The way that _FastAPI_ works is that anytime we send a request to our API server, it is going to find the **first match** for the requested route with the specific http method. So, the order of the methods does matter and is **VERY IMPORTANT** which we need to keep in mind when structuring our API with the routes and URLs.