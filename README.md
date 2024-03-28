# gmail-purger
gmail is almost full, time to cleanup:
- using python and gmail api
- get all the  email senders  in your email account
- identify sneder's whose email you can dicard entirely or by date range
- delete email by sender by date range

> I will be using poetry

## how to use poetry
- **Installation**: First, you need to install Poetry. After installation, make sure to add Poetry's bin directory to your PATH.
  You can do this by running the following command in your terminal:
  ```sh
  curl -sSL https://install.python-poetry.org | python3 -
  ```
- **Initialize Poetry:** Navigate to your project directory and initialize Poetry by running:
  ```sh
  poetry init
  ```
  Above command will guide you through creating a pyproject.toml file, which is used to manage your project's dependencies and settings.

- **Adding dependencies** to your project using the poetry add command. This command updates the pyproject.toml file and also
  creates a poetry.lock file to lock the versions of your dependencies.For example, **to add the requests library**, you would run:
  ```sh
  poetry add requests
  ```
  
- **Install Dependencies and create venv:** To install the dependencies specified in your pyproject.toml file, run:
  ```sh
  poetry install
  ```
  and this will create a virtual environment for your project and install the dependencies within it.

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
      
     - ** Remove the Virtual Environment**
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
