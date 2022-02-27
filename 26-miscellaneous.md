# Miscellaneous
## CORS
* CORS stands for Cross Origin Resource Sharing. When we send a request from a web browser to our API, our API does not allow that request. To undo that, we need to import something from FastAPI which can be followed along, in the [documentation](https://fastapi.tiangolo.com/tutorial/cors/?h=cor).
    * Command to run in inspect window -> console to check if we can access our API from the current origin (origin could be any website).
        ```
        fetch('http://localhost:8000/').then(res=>res.json()).then(console.log)
        ```

