# Steps to deploy app in ubuntu
* Create a Ubuntu VM
* Update all the packages in ubuntu  
* Ensure python and pip are installed 
* Install virtualenv
* Install and configure postgres
* Pull your code from github
* Set up environment variables and ensure that it is not lost if the system is rebooted.
* Create the database and set up the schema (with alembic).
* Create a service with gunicorn to run the app