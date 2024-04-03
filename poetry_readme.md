# poetry
**What is the right python packaging tool for you**

There are a number of tools available for managing dependencies in Python. In this article, we will compare most popular
tools: conda, pipenv, virtualenv, poetry, pyenv, and pip. Each have their own strengths and weaknesses, and the best
tool for you will depend on your specific needs.

- **Conda** is a more mature tool that has been around for longer. It is also more widely used, especially in the data science
community. Conda can be used to manage multiple Python versions, as well as other software packages. This makes it a
good choice if you need to use different versions of Python for different projects, or if you need to install software
packages that are not available in the Python Package Index (PyPI).

- **Pipenv** is a newer tool that combines the functionality of pip and virtualenv into one command-line interface. It is
designed to be more user-friendly than using pip and virtualenv separately, and it also includes features for managing
dependencies and creating project files.

- **Virtualenv** is a more established tool that is used to create isolated Python environments. This means that you can
install different packages in different virtual environments without affecting the packages installed in your global
Python environment. Virtualenv is a good choice if you need to create multiple isolated environments for different
projects.

- **Poetry** is a newer tool that is similar to Pipenv. It provides a more comprehensive solution for managing Python
packages, including features for dependency management, packaging, and distribution. Poetry is a good choice if you need
a tool that can handle all aspects of your Python project lifecycle.

- **Pyenv** is a tool that allows you to manage different Python versions on your system. This can be useful if you need to
use different versions of Python for different projects. Pyenv is not a dependency management tool, but it can be used
in conjunction with other tools like Pipenv or Poetry.

- **Pip** is the official package manager for Python. It is a simple and lightweight tool that can be used to install and
manage Python packages. Pip is a good choice if you are just getting started with Python, or if you need a basic tool
for managing dependencies.

### Comparison matrix
![img.png](img.png)

> Reference:
> https://www.linkedin.com/pulse/comparison-various-tools-manage-python-packages-virtual-mukesh-kumar/


## Install poetry
- **Installation**: First, you need to install Poetry. After installation, make sure to add Poetry's bin directory to your PATH.
  You can do this by running the following command in your terminal:
  ```shell
  # on macos
  brew install pipx
  pipx install poetry
  pipx ensurepath
  ```
  After installation, you should add the path to the Poetry executable to your system's PATH environment variable if it's not done automatically. 
  - Linux and macOS: ~/.local/bin/poetry
  - ls -l ~/.local/bin/poetry
    /Users/userid/.local/bin/poetry -> /Users/userid/Library/Application Support/pypoetry/venv/bin/poetry
  - echo 'export PATH="/Users/paramraghavan/Library/Application Support/pypoetry/venv/bin/poetry":$PATH' >> ~/.zshrc
  - Windows: %USERPROFILE%\.poetry\bin\poetry

  ```shell
  # show poetry version
  poetry --version
  ```

## Start brand new poetry project
>> Reference: https://python-poetry.org/docs/basic-usage/

First, let’s create our new project, let’s call it poetry-demo:
```shell
poetry new poetry-demo
```

This will create the poetry-demo directory with the following content:
```text
poetry-demo
├── pyproject.toml
├── README.md
├── poetry_demo
│   └── __init__.py
└── tests
    └── __init__.py
```

The pyproject.toml file is what is the most important here. This will orchestrate your project and its dependencies.
For now, it looks like this:

```text
[tool.poetry]
name = "poetry-demo"
version = "0.1.0"
description = ""
authors = ["Sébastien Eustace <sebastien@eustace.io>"]
readme = "README.md"
packages = [{include = "poetry_demo"}]

[tool.poetry.dependencies]
python = "^3.7"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

Poetry assumes your package contains a package with the same name as tool.poetry.name located in the root of your
project. If this is not the case, populate tool.poetry.packages to specify your packages and their locations.

Similarly, the traditional MANIFEST.in file is replaced by the tool.poetry.readme, tool.poetry.include, and
tool.poetry.exclude sections. tool.poetry.exclude is additionally implicitly populated by your .gitignore. For full
documentation on the project format, see the pyproject section of the documentation.

Poetry will require you to explicitly specify what versions of Python you intend to support, and its universal locking
will guarantee that your project is installable (and all dependencies claim support for) all supported Python versions.

### If you want to add dependencies to your project, you can specify them in the tool.poetry.dependencies section.
```text
[tool.poetry.dependencies]
pendulum = "^2.1"
```

As you can see, it takes a mapping of package names and version constraints. Poetry uses this information to search for
the right set of files in package “repositories” that you register in the tool.poetry.source section, or on PyPI by
default.

### Also, instead of modifying the pyproject.toml file by hand, you can use the add command.
```shell
$ poetry add pendulum
````

## Initialising a pre-existing project

Instead of creating a new project, Poetry can be used to ‘initialise’ a pre-populated directory. To interactively create
a pyproject.toml file in directory pre-existing-project:
```shell
cd pre-existing-project
poetry init
````
- **Adding dependencies** to your project using the poetry add command. This command updates the pyproject.toml file and also
  creates a poetry.lock file to lock the versions of your dependencies.For example, **to add the requests library**, you would run:
  ```sh
  poetry add requests
  ```

## If you venv is active use pip, you can update the toml file with the dependencies
**Dump current dependencies** dump out a pip list --format=freeze. This could help with resolving any tricky packages
that use different names in Conda versus PyPI.
```shell
pip list --format=freeze > curr_dependencies.txt
```
- **Initialize Poetry:** Navigate to your project directory and initialize Poetry by running:
  ```sh
  poetry init
  ```
Above command will guide you through creating a pyproject.toml file, which is used to manage your project's dependencies and settings.

**update the pyproject.toml**
Open the pyproject.toml and add all the dependencies from above file curr_depemdencies.txt  under the section
**[tool.poetry.dependencies]** in file pyproject.toml


## Lock dependencies and Install, Create Virtual env

- **Lock Dependencies:** Once you've added your dependencies,it will generate or update the poetry.lock file, locking
  the versions of your dependencies. It will also flag any dependency issues it finds. The poetry.lock file ensures that 
  everyone working on the project, or any environment where the project is deployed, will use the exact same versions of 
  dependencies, avoiding the "it works on my machine" problem. You can lock them with:
```shell
poetry lock
```

- **Install Dependencies and create venv:** To install the dependencies specified in your pyproject.toml file, run:
  ```sh
  poetry install
  ```
  and this will create a virtual environment for your project and install the dependencies within it.

## Running/Build/Publish Your Project
- **Running Your Project:** To run your project within the Poetry-managed virtual environment, you can
  use the poetry run command. For example:
  ```sh
  poetry run python main_script.py
  ```

- **Building and Publishing:** If you want to build your project into a distributable package, you can use:
```sh
  poetry build
```
- To publish your package to PyPI, use:
  ```sh
  poetry publish
  ```

 > Remember to commit the pyproject.toml and poetry.lock files to your version control
 > system to ensure consistent environments for all contributors to your project.   
     
## More options 
-  **List all virtual environments managed by Poetry**, you can use the following command:
```sh
poetry env list
```

- **Like pipenv clean**
  In Poetry, there is no direct equivalent to the pipenv clean command, which removes packages not specified in the Pipfile.
  However, you can achieve a similar result by using the poetry install command with the --remove-untracked option.

  This option will remove packages that are **not specified in the pyproject.toml** file from the virtual environment.
  Following will ensure that your virtual environment only contains the packages specified in your pyproject.toml file,
  similar to how pipenv clean works for Pipfile.
  ```sh
  poetry install --remove-untracked
  ```
- **Like pipenv install --dev**
   In Poetry, to install development dependencies, you use the poetry add command with the --dev flag. This is similar
   to how you would use pipenv install --dev in Pipenv. Here's an example:
    ```sh
    poetry add --dev pytest
    ```
    _Above command will add pytest as a development dependency to your project, updating the pyproject.toml and poetry.lock
    files accordingly._
      
- **Remove the Virtual Environment**
 Use the poetry env remove command to remove the current virtual environment. You need to specify the environment's name,
 which you can get from poetry env list.
 ```sh
  poetry env list  # Find the name of the current environment
  poetry env remove one_of_the_listed_envs  
 ```

- **Reinstall Dependencies**
 Run poetry install to create a new virtual environment and reinstall all dependencies (both default and development)
 based on the pyproject.toml and poetry.lock files.
  ```sh
  poetry install
  ```
- **path of the virtual environment associated with your project**
  Use the poetry env info command.
  ```sh
  poetry env info --path
  ```
- **List all the packages installed in your project's**
  ```sh
  #  list the packages installed in your project's virtual environment
  poetry show
  # This command will list all the packages installed in your project's virtual environment, along with their versions
  poetry show --tree
 ```

- **generate a requirements.txt**
 ```sh
   # to generate a requirements.txt file similar to using pipenv lock -r, you can use the poetry export
   poetry export -f requirements.txt --output requirements.txt
   # If you want to include only the main dependencies and exclude the development dependencies, you can add the --without dev
   poetry export -f requirements.txt --output requirements.txt --without dev
  ```

- **Visualize the dependency graph of your project (similar to pipenv graph)**
  ```sh
  poetry show --tree
  ```
  Above command will display a tree-like structure of your project's dependencies, showing how each package is related to
  others in terms of dependency. It's a useful way to understand the hierarchy and relationships between the packages in your project.

- **Execute pytest# 
  ```sh
  # To run pytest with the -vv flag on the test/ directory
  poetry run pytest -vv test/
  ```

- **Activate a virtual env**
  Navigate to the root directory of your project where the pyproject.toml file is located.
  Run the following command:
  ```sh
  poetry shell
  ```
  Above command will spawn a new shell with the virtual environment activated. You'll notice that the
  name of the virtual environment is usually displayed in your terminal prompt, indicating that the virtual environment is active.
  To deactivate the virtual environment and return to your original shell, you can type exit or press Ctrl + D.
    
- **Activate a virtual env from a list of  my configured envs**
 ```sh
 # list all venv's
 poetry env list
 
 ## output
 # my-project-py3.8 (Activated)
 # my-project-py3.9

 # exit from the current env using exit or ctrl+d 
 
 # switcching to my-project-py3.9
 poetry env use python3.9
 poetry shell
 ```
  
## poetry and requirements.txt
Here's how you can work with Poetry and requirements.txt:
Converting requirements.txt to Poetry: If you have an existing requirements.txt file and want to start using Poetry, you
can create a pyproject.toml file (which is used by Poetry) with all the dependencies listed in from requirements.txt
using the following command:

```sh
poetry init --no-interaction --dependency $(cat requirements.txt)
````
## Exporting dependencies to requirements.txt:
 If you're using Poetry but need a requirements.txt file for some reason (
e.g., for compatibility with tools that require it), you can export the dependencies from pyproject.toml to
requirements.txt using the following command:

```sh
poetry export -f requirements.txt --output requirements.txt
```


## To use a Conda environment YAML file with Poetry, you can follow these steps:

**Create and Activate Conda Environment:** First, create a Conda environment using the YAML file and activate it. This
ensures that the correct Python version and dependencies are used when setting up Poetry.

```sh
conda env create -f environment.yml
conda activate your_env_name
```

**Initialize Poetry:** In your project directory, initialize Poetry. This will create a pyproject.toml file for your
project. If you already have a pyproject.toml file, you can skip this step.

```sh
poetry init
```

During initialization, you can specify dependencies, but since you're using a Conda environment, you might want to skip
this and manually add dependencies later.

**Add Dependencies:** If there are additional dependencies that are not included in your Conda environment but are
required for your project, you can add them using Poetry. For example:

```sh
poetry add requests
```
Poetry will manage these dependencies separately from your Conda environment.

**Install Dependencies:** Run the following command to install the dependencies specified in your pyproject.toml file.
Since you're using a Conda environment, make sure it's activated before running this command.

```sh
poetry install
```

**Run Your Project:** With your Conda environment activated and Poetry set up, you can now run your project using the
Python interpreter from the Conda environment and dependencies managed by both Conda and Poetry.

By following these steps, you can use a Conda environment alongside Poetry for your project. This allows you to leverage
Conda for environment management and Poetry for dependency management.



## **Replace Conda Environment with poetry:** 
Activate Conda environment using the YAML file

```sh
conda env create -f environment.yml
conda activate your_env_name
```
- Got to section **If you venv is active use pip, you can update the toml file with the dependencies**, above
- ```shell
poetry install --remove-untracked
```


## Manage multiple virtual envs
```shell
poetry env use <env name>
poetry env info
poetry env list
poetry env remove <env name>
```
