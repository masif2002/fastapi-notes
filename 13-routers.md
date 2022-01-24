# Routers
As our program gets bigger, it also starts to look messy. So one of ways we can organize our main file is we can move our routes with our path operation functions to another folder called **routers** which will contain all the routes from now on.
* In the routers folder, we will have seperate files for seperate routes. 
    * For ex:
        * `@app.get('/posts')`
        * `@app.get('/posts/{id}')`
        * `@app.post('/posts')`
        * `@app.put('/posts/{id}')` and so on will be stored in a file called _posts.py_ since all the routes have something to deal with _posts_ and have a common prefix
    * And similary, any other routes that has to deal with users will be stored in *users.py* and so on.

* We need to create a router object for each route (_post.py_ and _user.py_ in our case). To do that, we first need to import **APIRouter** from the _FastAPI_ library. And then create the router object in our route files.
    ```python
    router = APIRouter()
    ```
    * We can actually add a prefix for the routes in this file by adding an extra parameter. We know that the routes in _post.py_ have common prefix of '/posts'. 
        ```w
        router = APIRouter(preifx = '/posts')
        ```
        * And now we can remove the extra '/posts' from the actual path in our path operation decorator.
* And then we replace the keyword _app_ we used in our decorator for the path operation with the name of the router object we just created.
    ```
    # Before
    @app.get("/posts/{id}")
    ```
    ```
    # After
    @router.get("/posts/{id}")
    ```

* And the next step is to reference our routers that we created in our _main.py_ file 
    * So, we import our route files first
        ```
        from .routers import user, post
        ```
* And then we include both of the routers in our application
    ```python
    app.include_router(post.router)
    app.include_router(user.router)
    ```
    * Note that, post.router is just the router object we created in _post.py_.
## Adding tags
You also can add another parameter while defining the route object. This parameter is called **tags**.
* This is basically just used for grouping routes in documentation to have a more organized look of all the routes in our application. 
* Routes with the same tags are grouped together.
    ```
    router = APIRouter(prefix='/posts', tags=['Posts'])
    ```
* The FastAPI auto-documentation for your program can be viewed by appending '_/docs_' to the URL of your website. This particular version of the documentation is powered using **SwaggerUI**. Howerever, there is also another version of the FastAPI documentation powered by **Redoc** which can be accessed by appending '/_redoc_' to the URL of your website.