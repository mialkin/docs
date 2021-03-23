# Python

## Virtual environment

Create and activate new virtual environment:

```bash
# python3 -m venv ENVIRONMENT_NAME
# source ENVIRONMENT_NAME/bin/activate
python3 -m venv venv
source venv/bin/activate
```

To deactivate environment type:

```bash
deactivate
```

Notes:

- The environment should be something that can be thrown away and rebuild. You don't want to put any of your project files in there.
- You shouldn't commit your environment into source control. What you would want to commit into your repository is `requirements.txt` file.

## Pip

| Command                         | Description                                                    |
| ------------------------------- | -------------------------------------------------------------- |
| pip freeze > requirements.txt   | Gets packages for `requirements.txt` file                      |
| pip3 install virtualenv         | Install virtual environment (may be not needed)                |
| pip install -r requirements.txt | Install packages into environment from `requirements.txt` file |
| pip list                        | List installed packages                                        |

## Python commands

| Command         | Description              |
| --------------- | ------------------------ |
| help('modules') | Prints installed modules |

## Web server

Start a new web server:

```console
python3 -m http.server
```

## Other

Print paths where Python is looking for modules:

```sh
import sys

print(sys.path)
```
