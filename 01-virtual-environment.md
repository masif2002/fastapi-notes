## Virtual Environment 
A virtual environment is an isolated environment which will not affect any other environment. Within this environment, we can install any python packages running whatever version we want and it is completely isolated to that particular project.

## Setting up a virtual environment in Windows

#### Step 1: Open the project folder in which we need to create a virtual environment 

#### Step 2: Open the terminal and type the following command in the command prompt

```
py -3 -m venv <name>
```
* Here, **venv** stands for virtual-environment
* And we can give any name of our choice for the virtual environment.

After the command is run, a new folder with the given name will be created in the project directory.

#### Step 3: Switch to the local interpreter

* python.exe in the Scripts folder is going to act as our new interpreter for the virtual environment. 
* Right now, we are using our global interpreter. So we need to change that to the specific one in our virtual environoment.
* To do that, in vscode, we need to go to **Views > Command Palette** and search for __Python: Select Interpreter__ and then we need to pass in the path of our new interpreter (ie- the one in our virtual environment).
```
.\name_of_venv\Scripts\python.exe
```

#### Step 4: Activate the virtual environment in the terminal

* We need to make sure that the terminal is also using the virtual environment. 
* To do that we are going to specify the path to **activate.bat** file in the _Scripts_ folder, in the command prompt.
```
venv\Scripts\activate.bat
```
Once this is done, we can see the name of the virtual environment in parantheses popping up on the side. By this we can make sure the virtual environment is activated.

## Starting FastAPI

First, we neeed to install the **FastAPI** package. 
```
pip install fastapi[all]
```
* This is going to install all of the optional dependecies aswell. 
* Make sure the command is run in the command prompt of the virtual environment.
* All these installed packages will be stores inside the Lib folder.

Once _FastAPI_ and all the dependencies has been installed, we import the package and initialise the app. And then we start our webserver in the cmd with the _uvicorn_ library .
```
uvicorn main:app
```
* Here __main__ refers to our file name, ie- **main.py**
* **app** refers to the name of our app we initialised in _main.py_.
* But here, we need to manually rerun the server if we make any changes. So, to avoid that we add an extra attribute `--reload` which automatically reruns the server once changes are made. (This is similar to setting `debug=True` while initialising an app with Flask)
```
uvicorn main:app --reload
```
If our _main.py_ is inside a package, we access it using a dot after the name of the package
```
uvicorn package.main:app -- reload
```