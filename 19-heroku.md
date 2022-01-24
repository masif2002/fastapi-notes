# Heroku 

## Setting up heroku
You can set up heroku for deplying your app by following along the tutorial [here](https://devcenter.heroku.com/articles/getting-started-with-python?singlepage=true).

## Starting our application
After setting up heroku following along the documentation, there are a few more steps that we need to follow before we can access our application.

* Heroku has no idea how to start our application. So we need to tell heroku how we can start our application. To do that, we need to create file called **Procfile** in the root directory of our project without any extension (like Procfile.txt).
    * The Procfile just tells heroku what is the command that is needed to start our application. 
    ```
    web: uvicorn app.main:app --host=0.0.0.0 --port =${PORT:-5000}
    ```
    * `uvicorn app.main:app` is how we normally start our application, locally. We access the _app_ object from the main file in our app directory (main.app).
    * `--host=0.0.0.0` make sures that we accept any IP that heroku gives us to run our app.
    * `--port=${PORT:-5000}` makes sure we use th port that heroku provides.

## Creating a postgres instance
Heroku porvides us with a free postgres instance that we can have access to. More information about postgres in heroku can be found [here](https://devcenter.heroku.com/articles/heroku-postgresql).

```
heroku addons:create heroku-postgresql:hobby-dev
```
* We run this command to create a postgres instance for us. hobby-dev is the name of the free plan that heroku offers us.

Then, we go to our heroku dahsboard on our web browser and click on our postgres instance and then, in settings we'll have our database credentials for the instance.
* After we have the credentials, we need to configure our environment variables that we defined in our application. Locally, we specified them in _env.py_, but now, we need to go to our heroku app (in the web browser) and then choose **Config Vars** in the settings.
    * There is already a env variable called DATABASE_URL that gets added by default when you create a postgres instance.
    * We can also use this variable in our program OR we can add the env variables in heroku that we configured in our own program (using pydantic).
* After setting the environment variables, we restart our heroku instance
    ```
    heroku ps:restart
    ```
## Setting up the tables in our database
We deployed a brand new instance of postgres to heroku. So, we haven't actually set up our tables.
* In our develpoment environment, alembic was responsible for creating our tables. 
* To get our development database upto date, we ran `alembic upgrade head` which would upgrade to the latest revision in alembic.
* Similarly, we are going to run the same command in our heroku instance which makes sure that our postgres database gets updated to our latest schema.
* Our heroku instance has access to all our alembic revision since the alembic directory is already checked into git.
* To run a command in heroku instance
    ```
    heroku run <command>
    ```
* Similarly, we run our command of upgrading our postgres instance to the latest revision with alembic
    ```
    heroku run alembic upgrade head
    ```
* Then, we restart our heroku instance
    ```
    heroku ps:restart
    ```
Aaand with that we're done deplying our application to Heroku :)