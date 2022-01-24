# Evironment varaiables
Environment variable is just a variable that you configure on your computer and any application that's running on that computer will be able to access it.

* We may forget to set any environmental variables that may cause our program to break. So, inorder to avoid that we can set up a Pydantic model for us to perform the validation, to check if all the required environmental variables have been set.
```py
from pydantic import BaseSettings

class Settings(BaseSettings):
    database_name: str
    database_hostname: str
    database_port: int
    database_username: str
    database_password: str
    secret_key: str
    algorithm: str
    access_token_expire_minutes: int

    class Config:
        env_file = ".env"
    
settings = Settings()
```
* Here, inside the class, we provide the list of all the environment variables that needs to be set
*  We can also give a default value for all these fields
* And an environment variable is always read as a string. Pydantic is the one which does the typecasting to the specified datatype
* Then, we create an instance of the class. And now, we can access all the variables with that instance. For example, 
    ```python
    print(settings.database_name)
    ```
    * This prints out the value of the environmental variable *database_name*. Similarly, all other environment variables can also be accessed.

Pydantic, when reading the enivornmental vairables from the system, it reads them from a case insensitive perspective, because the general convention for naming environment variables is ALL CAPS but we have them as all lowercase in our Pydantic model which wouldn't make a difference when using Pydantic.

## Environment file
* Instead of going into your system and changing the environment variables, we can do so in what is called an environment file for development purposes.
* We add the extra **_Config_** class to tell Pydantic to get all the environmental variables from the environment file '.env'
### .env
```
DATABASE_NAME = fastapi
DATABASE_HOSTNAME = localhost
DATABASE_PORT = 5432
DATABASE_USERNAME = postgres
DATABASE_PASSWORD = 1234
SECRET_KEY = 09d25e094faa6ca2556c818166b7a9563b93f7099f6f0f4caa6cf63b88e8d3e7
ALGORITHM = HS256
ACCESS_TOKEN_EXPIRE_MINUTES = 60
```
* This is how the values are stored in the environment file
