# Miscellaneous

## Python commands

| Command         | Description              |
| --------------- | ------------------------ |
| help('modules') | Prints installed modules |

## Pip

| Command                         | Description                                                |
| ------------------------------- | ---------------------------------------------------------- |
| pip freeze > requirements.txt   | Gets packages for `requerements.txt` file                  |
| pip3 install virtualenv         | Install virtual environment (may be not needed)            |
| pip install -r requirements.txt | Install packages into environment from `requerements` file |
| pip list                        | List installed packages                                    |

## Virtual environment

Create and activate new virtual environment:

```bash
python3 -m venv ENVIRONMENT_NAME
# Usually you will create virtual environment inside `venv` folder inside of your project:
# python3 -m venv PROJECT_NAME/venv
source ENVIRONMENT_NAME/bin/activate
```

To deactivate environment type:

```bash
deactivate
```

The version of Python that is going to be used is going to be the same as the version of Python that was used to create the environment:

```bash
which python        # Shows where Python that is being used is located
python --version
pip list
```

Notes:

-   The environment should be something that can be thrown away and rebuild. You don't want to put any of your project files in there.
-   You shouldn't commit your environment into source control. What you would want to commit into your repository is `requirements.txt` file.

## Web server

Start a new web server:

```console
python3 -m http.server
```

## Other

Make an alias for python3:

```sh
echo "alias python=python3" >> ~/.zshrc
```

Print paths where Python is looking for modules:

```sh
import sys

print(sys.path)
```
