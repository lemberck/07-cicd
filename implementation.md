## Implementation

## Add linters to poetry
- Start poetry env : `poetry shell`
- Add linters to dev env only : `poetry add --group dev pylint flake8`
- Install dependecies to the env : `poetry install --no-root`

### Run the linters for the python scripts
- **Flake8** : Returns a list of issues that it finds according to its rules. 
It is considered easier to integrate into a CI/CD pipeline due to its speed and simplicity.
    - Run for specific script : `poetry run flake8 path/to/python/scripts`
    - Run for all script in the project : `poetry run flake8 .`

> To **automate code checks with flake8 in a CI pipeline**, simply add it as a step in your workflow configuration. If flake8 reports any errors, the CI pipeline will fail, as it exits with a non-zero status upon finding linting violations.

> You can also configure flake8 using a **.flake8** file in your repository to customize the rules to fit the  project's guidelines. For example, you could ignore certain warnings or errors, exclude files, or adjust the maximum line length.

-----------------------------------------

- **Pylint** : It also provides a numerical score (up to 10) . pIt can be slower because it performs a full static analysis of the code, but has more checks than flake8. 
    - Run for specific script : `poetry run pylint path/to/python/scripts`
    - Run for all script in the project : `poetry run pylint folder_name/`
> The score can be used to **automate the linting process in the CI pipeline**, by setting a minimun score that must be met for the build to pass.

> You can add a **.pylintrc** file to customize the rules to fit the project's guidelines

#### --- After running the linters, solve the issues manually.


## Add unit tests [Not working yet, skip]
- Add test package to dev env only : `poetry add --group dev pytest pytest-asyncio`
- Create a test directory : `mkdir -p tests/backend tests/frontend`
- Create 'pytest.ini' at the root of the project, to show pytest to look for modules inside backend/ and frontend/ : `touch pyproject.ini`
  - https://pytest-with-eric.com/introduction/pytest-pythonpath/ 
```bash
[pytest]  
pythonpath = backend frontend
```

- Create tests for all scrips (the must start with 'test_') : `touch tests/backend/test_auth.py tests/backend/test_database.py tests/backend/test_models.py tests/backend/test_sentiment_analysis.py tests/frontend/test_streamlit_app.py`


## Add pre-commit (for future commits) [Not working yet, skip] 
Pre-commit is used to automate the enforcement of code style instead of just reporting. The framework can be set up to run the chosen linters automatically on every commit. With this, every time there is a commit, pre-commit will run the linters on the changed files. If there are any issues detected by the linters, the commit will be blocked until those issues are resolved. This helps to ensure that you maintain a high code quality.
- Add pre-commit as a Development Dependency: `poetry add --group dev pre-commit`
- Create the pre-commit Configuration File: 
    - Create a file named .pre-commit-config.yaml with the following content to configure pre-commit to run flake8 and pylint: 
    ```bash 
    repos:
  - repo: https://github.com/pycqa/flake8
    rev: ''  # Use the desired version of flake8
    hooks:
      - id: flake8

  - repo: https://github.com/pycqa/pylint
    rev: ''  # Use the desired version of pylint
    hooks:
      - id: pylint
        additional_dependencies: ['pylint']
    ```
    **Note** : To check the installed versions of flake8 and pylint : `poetry show | grep -E "flake8|pylint"`

- Install the pre-commit Hook: `poetry run pre-commit install`
- Run pre-commit Against All Files (Optional - Recommended): `poetry run pre-commit run --all-files`
- Commit The Changes:
    - Add the .pre-commit-config.yaml and any changes to pyproject.toml to your repository and commit them:
    ```bash
    git add .pre-commit-config.yaml pyproject.toml
    git commit -m "Add pre-commit hooks for flake8 and pylint"
    ```

## CI/CD Workflow
