# venv 

Creates isolated Python environment for projects. 

## How to create a new venv ? :
    python3 -m venv /path/to/venv 
## How to activate/deactivate a venv ? :
    source /path/to/venv/bin/activate 
*To deactivate, typing `deactivate`should be enough*
## How to delete a venv ? :
    rm -rf /path/to/venv 
## How to copy a venv configuration ? :
*From the activate venv we want to copy:*

    `pip freeze > requirements.txt`
    `deactivate`
*Then create the new venv as seen previously, activate it and :*

    `pip install -r requirements.txt`
 
*Note : the requirements.txt get saved in the current working directory. So not necessarily in the venv folder*

# pip/pipx 

pip : For installing packages in the current environment (global or virtual).
pipx : For installing and managing Python-based **command-line tools** globally but in isolated environments.

## How to use for a venv a globally installed package (jupyter) ?
*First we have to install globally the package : *
```
    pipx install jupyter
```
 *Then we need to register the venv as a Jupyter kernel:*
```
     source path/to/venv/bin/activate
     pip install ipykernel
     python -m ipykernel install --user --name=my_venv_kernel --display-name "Python (my_venv)"
```

*In Jupyter interface, we will now be able to select the kernel, depending on which venv we are working.
This approach allow to install jupyter once rather than in each venv. *


