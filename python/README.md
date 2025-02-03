# venv 

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


