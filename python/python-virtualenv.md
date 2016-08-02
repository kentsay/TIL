# Python Dev environment setup

### Install pip
`pip` is a package management system used to install and manage software packages written in Python. Check [here](https://pip.pypa.io/en/stable/installing/) to see how to install pip on your machine.

### Virtual Environments
A Virtual Environment is a tool to keep the dependencies required by different projects in separate places, by creating virtual Python environments for them. It solves the “Project X depends on version 1.x but, Project Y needs 4.x” dilemma, and keeps your global site-packages directory clean and manageable.

For example, you can work on a project which requires Django 1.3 while also maintaining a project which requires Django 1.0.

```
$ pip install virtualenv
```

Now you can create your own virtualenv in your project:
```
$ virtualenv venv
```
This command will create a folder in the current directory which will contain the Python executable files, and a copy of the pip library which you can use to install other packages.

To begin using the virtual environment, it needs to be activated:
```
$ source venv/bin/activate
```
Now you can start installing the package you need for this project.

### Other notes

In order to keep your environment consistent, it’s a good idea to “freeze” the current state of the environment packages. To do this, run
```
$ pip freeze > requirements.txt
```

This will create a requirements.txt file, which contains a simple list of all the packages in the current environment, and their respective versions. You can see the list of installed packages without the requirements format using “pip list”. Later it will be easier for a different developer (or you, if you need to re-create the environment) to install the same packages using the same versions:
```
$ pip install -r requirements.txt
```
This can help ensure consistency across installations, across deployments, and across developers.

Lastly, remember to exclude the virtual environment folder from source control by adding it to the ignore list.

### virtualenvwrapper

virtualenvwrapper provides a set of commands which makes working with virtual environments much more pleasant. It also places all your virtual environments in one place.

To install (make sure virtualenv is already installed):
```
$ pip install virtualenvwrapper
$ export WORKON_HOME=~/Envs
$ source /usr/local/bin/virtualenvwrapper.sh
```

### Basic usage

* Create a virtual environment:
```
$ mkvirtualenv venv
```
* Work on a virtual environment:
```
$ workon venv
```
* Alternatively, you can make a project, which creates the virtual environment, and also a project directory inside `$PROJECT_HOME`, which is `cd` -ed into when you `workon myproject`.
```
$ mkproject myproject
```

* Deactivating is still the same:
```
$ deactivate
```

* To delete:
```
$ rmvirtualenv venv
```

### Reference
* [python virtual env](http://docs.python-guide.org/en/latest/dev/virtualenvs/)
