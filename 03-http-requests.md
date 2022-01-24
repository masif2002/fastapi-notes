# HTTP Requests
There are a variety of HTTP requests which can be found [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods). Some of them are:
* **GET** - Used to retrieve data
* **POST** - Used to send data to the API server

# Postman
Until now, we used our web browser to generate HTTP requests to test our API. That's fine until, we want to send more http requests like PUT, POST, PATCH or any other methods that involves sending data to our API because it gets more complex because there is no way to natively do that in our browser without builiding a front-end application.
* But, it doesn't make sense to develop a frontend application ONLY to test an API.
*  So, there are a lot of tools available for testing an API. One of those tools is called **POSTMAN**. 
    * It is a very simple tool that allows us to construct our own HTTP requests.
    * We can download it [here](https://www.postman.com/downloads/).
     
## Sending data in POST requests using postman
 We send data in the body of the request. To do that in postman, we need to go to **Body > raw** and select the type of data to **JSON**.
* JSON operates very similar to a Python dictionary. Bsically, it contains a bunch of key-value pairs.
```
{
    'title': 'My first Post',
    'content': 'API is fun'
}
```

## Retrieving data from POST requests

Within our path operation function, we can assign a variable that'll hold the data from the  _body_ of the request. Here we give the name of the variable as _payload_, however it can be anything.
```python
@app.post("/createpost")
def create_post(payload: dict = Body(...)):
    print(payload)
    return {"message": "Post has been created"}
```
* This piece of code basically extracts all of the fields from the **Body** which is going to be converted to a Python **dictionary** and it is going to be stored in the variable **payload**.
* The ... inside `Body()` signifies that we extract all the fields from the body
* The `Body()` needs to be imported from _fastapi.params_ but vscode automatically imports it once we use it in our code.